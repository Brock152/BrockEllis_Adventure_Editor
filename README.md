import json
"""
Created on Wed Feb 21 12:04:58 2024

@author: uball
"""

def main():
    game = getDefaultGame()
    keepGoing = True
    while keepGoing:
        userChoice = getUserChoice()
        if userChoice == "0":
            keepGoing = False
        elif userChoice == "1":
            print("load default game")
            game = getDefaultGame() 
        elif userChoice == "2": 
            print("load a game file")
            game = loadGame()
        elif userChoice == "3":
            print("save the current game")
            saveGame(game)
        elif userChoice == "4":
            print("edit or add node")
            game = editNode(game)
        elif userChoice == "5":
            print("Good luck")
            playGame(game)
        else:
            print("something went wrong here")
 

def getUserChoice():
    """ prints a new menu, gurantees a result string 0-5"""

    print("""
      0) exit
      1) load default game
      2) load a game file
      3) save the current game
      4) edit or add a node
      5) play the current """) 
    userChoice = input(" what will you do? ")
    return userChoice

def getDefaultGame():
    """ creates the simplest possible defualt game """
    game = {"start": ["default start node", "Start over", "start", "Quit", "quit"]}
    return game 

def saveGame(game): 
    """ uses JSON module to print out game and save it to a file """
    fileOut = open("game.json", "w")
    print(json.dumps(game, indent =2))
    json.dump(game, fileOut, indent =2)
    fileOut.close()
    
def loadGame(): 
    """ uses JSON module to Load Game from game.json """
    fileIn = open("game.json", "r")
    game = json.load(fileIn)
    fileIn.close()
    return game
    
def playGame(game): 
    """ plays the game """
    keepGoing = True
    currentNode = "start"
    while keepGoing:
        if currentNode == "quit":
            keepGoing = False
        else: 
            currentNode = playNode(game, currentNode)
            
def playNode(game, currentNode):
    """ plays the node if possible, 
        returning new node or 'quit'
        if currentNode is not in game """
    if currentNode in game.keys():
        (desc, menuA, nodeA, menuB, nodeB) = game[currentNode]
        print(f"""
        {desc}
        1) {menuA}
        2) {menuB}
        """)
        userChoice = input("Which do you choose? (1/2) ")
        if userChoice == "1":
            nextNode = nodeA
        elif userChoice == "2": 
            nextNode = nodeA 
        else:
            print("Please enter 1 or 2")
            nextNode
    else: 
            #currentNode is NOT a node name in the game
            print("That was not a legal node. Quitting the game.")
            nextNode = "quit"
            
    return nextNode

def editNode(game):
    """ lists the current game so you can see all the nodes,
        allows you to pick a node to edit
        if it's an existing node, edit it
        if it's a new node, create it as blank then edit """
        
    print("current status of game:")
    print(json.dumps(game, indent=2))

    print("Existing node names: ")
    for nodeName in game.keys(): 
        print(f"  {nodeName}") 

    newNodeName = input("name of node to edit or create? ")
    if newNodeName in game.keys():
        newContent = game[newNodeName]
    else:
        newContent = ["", "", "", "", ""]
        print("Nodes needed: ")
        
    (desc, menuA, nodeA, menuB, nodeB) = newContent
    
    newDesc = getField("Description", desc)
    newMenuA = getField("Menu A", menuA)
    newNodeA = getField("Node A", nodeA)
    newMenuB = getField("Menu B", menuB)
    newNodeB = getField("Node B", nodeB)
    


    game[newNodeName] = [newDesc, newMenuA, newNodeA, newMenuB, newNodeB]

    return game

def getField(prompt, currentVal):
    """ prompts for a field if
        user presses any key, keep current value """
        
    newVal = input(f"{prompt} ({currentVal}): ")
    if newVal == "":
        newVal = currentVal
        
    return newVal

if __name__ == "__main__":
    main() 
