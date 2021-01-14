# Implement new command

## Command without subcommand

There are few steps to creating a new command in MolTwister. These are as follows.

Create a class that subclasses from CCmd. An example is shown below for the header file, which in this case should be called MolTwisterCmdMyCmd.h.
````c++
#pragma once
#include <string>
#include "MolTwisterCmd.h"

class CCmdMyCmd : public CCmd
{
public:
    CCmdMyCmd() = delete;
    CCmdMyCmd(CMolTwisterState* state) : CCmd(state) { state_ = state; }
    
public:
    void execute(std::string commandLine);
    std::string getCmd() const { return "mycmd"; }
    std::string getTopLevHelpString() const { return std::string("This command does what I want"); }
    std::string getHelpString() const;
    
protected:
    virtual void onAddKeywords();
};
````
The corresponding implementation file should be called MolTwisterCmdMyCmd.cpp and would have the structure shown below.
````c++
#include "MolTwisterCmdMyCmd.h"

void CCmdMyCmd::onAddKeywords()
{
    addKeyword("mycmd");
    addKeyword("dothis")
    addKeyword("dothat")
}

std::string CCmdMyCmd::getHelpString() const
{ 
    std::string text;
    
    text+= "\tUsage: mycmd <selection>\r\n";
    text+= "\r\n";
    text+= "\tThis command has two options that can be chosen by\r\n";
    text+= "\tsetting <selection>=dothis or <selection>=dothat.";

    return text;
}

void CCmdMyCmd::execute(std::string commandLine)
{
    int arg = 1

    std::string text = CASCIIUtility::getWord(commandLine, arg++);
    if(text == "dothis")
    {
        // Do this!
    }
    if(text == "dothat")
    {
        // Do that!
    }
}
````
Both these files should be stored under the Cmd folder. Once the command class has been created it must be added into the CMakeLists.txt 
file. This is done by adding entries into the MT_SOURCE_FILES variable:
````text
        .
        .
        .
    ${CMAKE_CURRENT_LIST_DIR}/Cmd/MolTwisterCmdVarlist.h
    ${CMAKE_CURRENT_LIST_DIR}/Cmd/MolTwisterCmdVarlist.cpp
    ${CMAKE_CURRENT_LIST_DIR}/Cmd/MolTwisterCmdMolDyn.h
    ${CMAKE_CURRENT_LIST_DIR}/Cmd/MolTwisterCmdMolDyn.cpp
    ${CMAKE_CURRENT_LIST_DIR}/Cmd/MolTwisterCmdMyCmd.h
    ${CMAKE_CURRENT_LIST_DIR}/Cmd/MolTwisterCmdMyCmd.cpp
        .
        .
        .
````

Note that the CCmd class contains the state_ attribute, which is a pointer 
to CMolTwisterState. This contains information about the entire state of the molecular system. From this it is also possible to
manipulate these states (such as adding, deleting and modifying atoms, as well as atomic force-fields). For example, an important 
public attribute of the CMolTwisterState class is atoms_, which is a vector of pointers to all atoms defined in the system. It is
helpful to study other commands in the system to get familliar with how this class works. Another useful class to know about
is the CDCDFile class, located under the Utilities folder. This can be used to access single records within a DCD file. This is
also extensively used within other commands.

After the command had been created and added to the build process, it needs to be instantiated and registered. This is done in the
CMolTwisterCommandPool class. To add the command, include the header file and add a registration of the command in the implementation 
file:
````c++
#include "MolTwisterCommandPool.h"

#include "Cmd/MolTwisterCmdCd.h"
    .
    .
    .
#include "Cmd/MolTwisterCmdMyCmd.h"

void CMolTwisterCommandPool::generateCmdList(CMolTwisterState* mtState, std::vector<std::shared_ptr<CCmd>>& cmdList)
{
    cmdList.emplace_back(std::make_shared<CCmdCd>(mtState));
        .
        .
        .
    cmdList.emplace_back(std::make_shared<CCmdMycmd>(mtState));

    for(size_t i=0; i<cmdList.size(); i++) cmdList[i]->init();
}
````

Now MolTwister should show the command under 'help' and it should be possible to execute the command. Auto-complete
will be available for all keywords added with the addKeyword() method.

## Command with subcommand

It is also possible to create commands where each sub-command of the main command is its own class. The 'measure' and
'calculate' commands are of this kind. In that case the main command header file is implemented in the following manner.
````c++
#pragma once
#include <string>
#include "MolTwisterCmd.h"

class CCmdMyCmd : public CCmd
{
public:
    CCmdMyCmd() = delete;
    CCmdMyCmd(CMolTwisterState* state) : CCmd(state) { state_ = state; }
    
public:
    virtual void execute(std::string commandLine);
    virtual std::string getCmd() const { return "mycmd"; }
    std::string getTopLevHelpString() const { return std::string("his command does what I want"); }
    std::string getHelpString() const;
    std::string getHelpString(std::string subCommand) const;

protected:
    virtual void onAddKeywords();
    virtual void onRegisterSubCommands();
};
````
As can be seen, compared to before, the getHelpString() has a second override and onRegisterSubCommands() is overriden.
The implementation file for the main command looks as follows.
````c++
#include <stdio.h>
#include "MolTwisterCmdMyCmd.h"

#include "MolTwisterCmdMyCmd/CmdMySub1.h"
#include "MolTwisterCmdMyCmd/CmdMySub2.h"

void CCmdMyCmd::onAddKeywords()
{
    addKeyword("mycmd");
    addKeywords(parser_->getCmdLineKeywords());
}

void CCmdMyCmd::onRegisterSubCommands()
{
    parser_->purge();
    parser_->registerCmd(std::make_shared<CCmdMySub1>(state_, stdOut_));
    parser_->registerCmd(std::make_shared<CCmdMySub2>(state_, stdOut_));
}

std::string CCmdMyCmd::getHelpString() const
{
    return parser_->genHelpText(getCmd());
}

std::string CCmdMyCmd::getHelpString(std::string subCommand) const
{
    return parser_->genHelpText(getCmd(), subCommand);
}

void CCmdMyCmd::execute(std::string commandLine)
{
    std::string lastError = parser_->executeCmd(commandLine);
    if(!lastError.empty())
    {
        printf("%s\r\n", lastError.data());
        return;
    }

    // requestUpdate() is only needed if any of the commands require
    // updating of the 3D view contents!
    if(state_->view3D_)
    {
        if(state_->atoms_.size() == 1) state_->view3D_->requestUpdate(true);
        else                           state_->view3D_->requestUpdate(false);
    }
}
````
The header and implementation files for the main command should be stored under the same folder as for a command without subcommands.
Each subcommand class should be implemented in its own header and implementation file pair and stored under a folder with the same
name as the files containing the main command. In this case that would be a folder with name MolTwisterCmdMyCmd.

The header file of a subcommand has the following structure
````c++
#pragma once
#include "../Tools/MolTwisterCmdEntry.h"

class CCmdMySub1 : public CCmdEntry
{
public:
    CCmdMySub1() = delete;
    CCmdMySub1(CMolTwisterState* state, FILE* stdOut) : CCmdEntry(state, stdOut) { }
    virtual ~CCmdMySub1() = default;

public:
    std::string getCmd();
    std::vector<std::string> getCmdLineKeywords();
    std::vector<std::string> getCmdHelpLines();
    std::string getCmdFreetextHelp();
    std::string execute(std::vector<std::string> arguments);

private:
    std::string lastError_;
};
````
where the file name of the header file would be CmdMySub1.h. The corresponding implementation file, CmdMySub1.cpp, has the structure
shown below.
````c++
#include "CmdMySub1.h"
#include "../../Utilities/ASCIIUtility.h"

std::string CCmdMySub1::getCmd()
{
    return "mysub1";
}

std::vector<std::string> CCmdMySub1::getCmdLineKeywords()
{
    return { "mysub1", "dothis", "dothat" };
}

std::vector<std::string> CCmdMySub1::getCmdHelpLines()
{
    return {
                "mysub1 <selection>"
           };
}

std::string CCmdMySub1::getCmdFreetextHelp()
{
    std::string text;

    text+= "\tThis subcommand has two options that can be chosen by\r\n";
    text+= "\tsetting <selection>=dothis or <selection>=dothat.";

    return text;
}

std::string CCmdMySub1::execute(std::vector<std::string> arguments)
{
    lastError_ = "";
    size_t arg = 0;

    std::string text = CASCIIUtility::getArg(arguments, arg++);
    if(text == "dothis")
    {
        // Do this!
    }
    else if(text == "dothat")
    {
        // Do that!
    }
    else
    {
        lastError_ = "Error: unknown selection!";
        return lastError_;
    }

    return lastError_;
}
````
To add more subcommands, add a new subcommand class for each, and register them (e.g., CCmdMySub2, which was registered in the example 
main command of this section).

Note that several versions of the subcommand can be registered in getCmdHelpLines(), separated by commas, where the available versions 
are determined by the implementation in execute().
