### For every function you wrote (TODO 1 to 19), explain in your own words what it does and give its time complexity in terms of n, the number of scenes. One short paragraph each.
== TODO 1 (Scene constructor): If numChoices is more than 0, it makes an array of that many Scene pointers and sets each slot to nullptr. Endings get no array. O(1), since there are at most 3 slots.

TODO 2 (~Scene): Deletes every child scene, then the choices_ array, then data_. Because each child’s destructor does the same, the whole subtree is freed. O(n) for the whole tree, O(1) of its own work per scene.

TODO 3 (isEnding): Returns true if numChoices_ is 0. O(1).

TODO 4 (getChoice): Returns the scene in slot index, or nullptr if the index isn’t a real slot. O(1).

TODO 5 (setChoice): Puts next into slot index and does nothing if the index is out of range. O(1).

TODO 6 (countScenes): Counts 1 for this scene plus the count of each child, recursively, and returns 0 for nullptr. It visits every scene once, so O(n).

TODO 7 (countEndings): Returns 1 at an ending and otherwise adds up the endings under each child. O(n).

TODO 8 (height): Returns the tallest child’s height plus 1. An ending is 0 and an empty tree is -1. O(n).

TODO 9 (clear): Deletes root_, which frees everything through the destructor, then sets root_ to nullptr. O(n).

TODO 10 (load): Opens the file, reads START_HP, and calls readScene to build the tree. It returns false if the file won’t open or readScene fails, and otherwise gives the tree to setRoot. O(n), since each scene is read once.

TODO 11 (readScene): Reads the scene’s fields, checks the choice count, and makes a Moment and a Scene. Then it reads a LABEL and recursively reads each child, and on any error it deletes what it built and returns nullptr. O(n).

TODO 12 (askChoice): Prompts, reads a line, and checks that it’s all digits and in range. It repeats until the input is valid and returns 0 if input ends. O(1) per attempt.

TODO 13 (play): Starts at the root and enters one scene at a time. Each scene updates HP and loot, prints the text, handles the item, and checks for game over or an ending. Otherwise it prints the choices and moves to the one the player picks. It follows one root-to-leaf path, so it takes at most height + 1 steps, which is O(n). Each bag search adds O(k), where k is the bag size, so the worst case is O(h·k).

TODO 14 (printStats): Prints the scene count, ending count, and height using the tree functions. O(n).

TODO 15 (printSpoilers): Calls printEndings from the root with an empty path. O(n).

TODO 16 (printEndings): Prints the path and text at each ending and otherwise recurses into each choice with the path extended. Choice 1 goes first, then 2, then 3. O(n).

TODO 17 (greedyRun): Walks down from the root and always takes the choice with the most loot, ties going to the lowest number. It stops on death or at an ending. It follows one path, so O(h), which is at most O(n).

TODO 18 (bestLoot): Returns the best loot from this scene down to an ending, or -1 if every path kills you. It tries every child and keeps the best one with its path. It visits every scene once, so O(n).

TODO 19 (optimalRun): Calls bestLoot from the root and prints the best path and loot, or the “no path survives” message. O(n).

### Why does each scene allocate exactly numChoices slots instead of declaring Scene<T>* choices[3]? What does an ending hold in choices_, and why?
== Scene<T>* choices[3] would waste memory on every scene that has fewer than 3 choices, especially endings which need none. By allocating exactly numChoices slots uses only the memory a scene needs.