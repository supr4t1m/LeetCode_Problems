1948\. Delete Duplicate Folders in System
==========================================

## Intuition
To tackle the problem of identifying and removing duplicate folders in a file system, we can leverage a tree-like structure to represent the folder hierarchy. By using a Trie (prefix tree), we can efficiently store and compare folder paths. The key idea is to serialize the structure of each folder and its subfolders, allowing us to identify duplicates based on their serialized representation.

## Approach
1. **Trie Structure**: Define a `Trie` structure that contains a string for serialization and a map to hold its children folders.
2. **Building the Trie**: Iterate through each path in the input and construct the Trie. Each folder in the path becomes a node in the Trie.
3. **Frequency Calculation**: Perform a post-order traversal of the Trie to calculate the frequency of each folder structure. During this traversal, serialize the structure of each folder by concatenating the names of its children and their serialized representations.
4. **Identifying Duplicates**: Use a frequency map to track how many times each serialized structure appears. If a structure appears more than once, it indicates that the folder and its subfolders are duplicates.
5. **Collecting Remaining Paths**: Perform another traversal of the Trie to collect paths that are not marked for deletion (i.e., those with a frequency of 1).
6. **Memory Cleanup**: After processing, free the memory associated with the Trie to prevent memory leaks.

## Complexity
- **Time complexity**: The time complexity is $$O(n \cdot m)$$, where $$n$$ is the number of paths and $$m$$ is the average number of folders in each path. This accounts for building the Trie and traversing it to calculate frequencies and collect remaining paths.
  
- **Space complexity**: The space complexity is also $$O(n \cdot m)$$, as we need to store the Trie structure and the frequency map.

## Code
```cpp
struct Trie {
    string serial;
    unordered_map<string, Trie*> children;
};

class Solution {
public:
    vector<vector<string>> deleteDuplicateFolder(vector<vector<string>>& paths) {
        // root
        Trie* root = new Trie();

        // build the trie from the 2d array
        for (const auto& path: paths) {
            Trie* curr = root;
            
            for (string node: path) {
                if (!curr->children.count(node)) {
                    curr->children[node] = new Trie();
                }
                curr = curr->children[node];
            }
        }

        // hashmap to store the frequency of folder structure
        unordered_map<string, int> freq;

        // post order traversal of Trie to calculate frequency
        // and serial
        function<void(Trie*)> construct = [&](Trie* node) {
            if (node->children.empty())
                return;

            vector<string> v;
            for (const auto& [folder, child]: node->children) {
                construct(child);
                v.push_back(folder + "(" + child->serial + ")");
            }

            sort(v.begin(), v.end());
            
            // to avoid difference due to ordering
            for (string s: v)
                node->serial += s;

            ++freq[node->serial];
        };

        construct(root);

        vector<vector<string>> ans;

        // array to store the current path being traversed
        vector<string> path;

        // traversal to delete duplicate paths
        function<void(Trie*)> operate = [&](Trie* node) {
            if (freq[node->serial] > 1)
                return;

            if (!path.empty())
                ans.push_back(path);

            for (const auto& [folder, child]: node->children) {
                path.push_back(folder);
                operate(child);
                path.pop_back();
            }
        };

        operate(root);

        // free the memory associated with Trie
        function<void(Trie*)> cleanUp = [&](Trie *node) {
            for (const auto& [folder, child]: node->children) {
                cleanUp(child);
            }
            delete node;
        };

        cleanUp(root);

        return ans;
    }
};
```
