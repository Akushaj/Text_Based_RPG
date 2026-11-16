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

### Walk through your Scene destructor. What gets deleted, and in what order? What would go wrong if choices_ were deleted first?
== It first deletes each child scene in choices_[i], which recursively deletes that child’s whole subtree. Then it does delete[] choices_ to free the array of pointers, and last delete data_ to free the Moment. If choices_ were deleted first, we’d lose the pointers to the children. Nothing could free those scenes anymore, so every child and everything under it would leak.

## Show the path greedy took and the path optimal took on your story, with the loot for each. Explain why greedy lost. If greedy did not lose, change your story until it does, and explain what you changed.
== Greedy: path 2-1-1-1-1, loot 10, and it died.
Optimal: path 2-1-1-1-2-1-1-1, loot 25, and it reached an ending.
Greedy lost because it only looks at the next scene’s loot. At the fifth choice it took the bigger-loot option, which cost too much HP and killed it. Optimal took the lower-loot option, stayed alive, and collected more loot later. Since greedy already lost, I didn’t have to change my story.

###  bestLoot tries every path, yet it runs in O(n) time. Explain why it is not exponential, using what you know about paths and leaves in a tree.
== It does try every path, but the paths share their starting scenes, so we never redo that work. Each scene is visited exactly once, and the function does a constant amount of work per scene. The number of root-to-leaf paths equals the number of leaves, which is at most n. So the total work is proportional to the number of scenes, O(n), not exponential.

### Which List methods does your game use, and why? What does one search cost as the bag grows ?
== The game uses addFront to add a new item. Search to check if the bag already has an item. Size for counting inventory. Print to show the bag at the end.In the worst case, when the item isn’t in the bag, it checks k items, so one search costs O(k), where k is the number of items in the bag. As the bag grows, searching for an item can take longer because there are more items to check.
