# Problem of the Week – Autocomplete System  
Company: Twitter  
Difficulty: Medium  
Topic: Strings, Trie, Hashing  

## Problem Statement  
Implement an autocomplete system that, given a query prefix and a set of dictionary strings, returns all words that begin with that prefix.

### Approaches  
1. Brute Force Search  
   - Iterate through all words in the dictionary.  
   - Check if each starts with the prefix.  
   - Time Complexity: O(N * L) (N = number of words, L = length of prefix).  

2. Trie (Prefix Tree) Approach – Efficient  
   - Preprocess dictionary into a Trie.  
   - Traverse the Trie using the prefix.  
   - Perform DFS/BFS from the node to collect words.  
   - Lookup Time: O(L + K) (L = prefix length, K = number of results).  

## Java Code  
```java
import java.util.*;

class TrieNode {
    Map<Character, TrieNode> children = new HashMap<>();
    boolean isEndOfWord = false;
}

public class AutocompleteSystem {
    private TrieNode root;

    public AutocompleteSystem() {
        root = new TrieNode();
    }

    public void insert(String word) {
        TrieNode node = root;
        for (char ch : word.toCharArray()) {
            node.children.putIfAbsent(ch, new TrieNode());
            node = node.children.get(ch);
        }
        node.isEndOfWord = true;
    }

    public List<String> autocomplete(String prefix) {
        List<String> results = new ArrayList<>();
        TrieNode node = root;
        for (char ch : prefix.toCharArray()) {
            if (!node.children.containsKey(ch)) {
                return results; // no words with this prefix
            }
            node = node.children.get(ch);
        }
        dfs(node, new StringBuilder(prefix), results);
        return results;
    }

    private void dfs(TrieNode node, StringBuilder path, List<String> results) {
        if (node.isEndOfWord) {
            results.add(path.toString());
        }
        for (Map.Entry<Character, TrieNode> entry : node.children.entrySet()) {
            path.append(entry.getKey());
            dfs(entry.getValue(), path, results);
            path.deleteCharAt(path.length() - 1);
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        AutocompleteSystem ac = new AutocompleteSystem();
        for (int i = 0; i < n; i++) {
            ac.insert(sc.next());
        }
        String prefix = sc.next();
        List<String> results = ac.autocomplete(prefix);
        for (String word : results) {
            System.out.print(word + " ");
        }
    }
}
