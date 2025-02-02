Step 1: Deleting a Node in a Binary Search Tree (BST)
To implement a delete method in a Binary Search Tree, you have to consider several different cases, because deleting a node can have different outcomes based on the structure of the tree. Here's an outline of the different cases:

Node to delete is a leaf (no children).

Just remove the node.
Node to delete has one child.

Replace the node with its child.
Node to delete has two children.

Find the inorder successor (smallest node in the right subtree) or the inorder predecessor (largest node in the left subtree) and replace the node to delete with this successor or predecessor. Then delete the successor or predecessor, which will fall into one of the first two cases.


Step 2: Code Implementation
#include <iostream>
using namespace std;

// Define the structure of a Node
class Node {
public:
    int value;
    Node* left;
    Node* right;

    Node(int key) {
        value = key;
        left = nullptr;
        right = nullptr;
    }
};

// Define the Binary Search Tree class
class BinarySearchTree {
public:
    Node* root;

    BinarySearchTree() {
        root = nullptr;
    }

    // Insert method to insert a new value into the BST
    Node* insert(Node* root, int key) {
        if (root == nullptr) {
            return new Node(key);
        }
        if (key < root->value) {
            root->left = insert(root->left, key);
        } else {
            root->right = insert(root->right, key);
        }
        return root;
    }

    // Delete method to delete a node from the BST
    Node* deleteNode(Node* root, int key) {
        // Base case: if the root is null, return null
        if (root == nullptr) {
            return root;
        }

        // If key to be deleted is smaller than the root's value, it lies in the left subtree
        if (key < root->value) {
            root->left = deleteNode(root->left, key);
        }
        // If key to be deleted is greater than the root's value, it lies in the right subtree
        else if (key > root->value) {
            root->right = deleteNode(root->right, key);
        }
        // If key is the same as root's value, this is the node to delete
        else {
            // Case 1: Node has no children (leaf node)
            if (root->left == nullptr && root->right == nullptr) {
                delete root;
                return nullptr;
            }
            // Case 2: Node has one child
            else if (root->left == nullptr) {
                Node* temp = root->right;
                delete root;
                return temp;
            }
            else if (root->right == nullptr) {
                Node* temp = root->left;
                delete root;
                return temp;
            }
            // Case 3: Node has two children
            else {
                // Find the inorder successor (smallest node in the right subtree)
                Node* temp = minValueNode(root->right);

                // Copy the inorder successor's value to this node
                root->value = temp->value;

                // Delete the inorder successor
                root->right = deleteNode(root->right, temp->value);
            }
        }
        return root;
    }

    // Helper method to find the node with the smallest value (inorder successor)
    Node* minValueNode(Node* node) {
        Node* current = node;
        while (current && current->left != nullptr) {
            current = current->left;
        }
        return current;
    }

    // Function to do inorder traversal and print the tree
    void inorder(Node* root) {
        if (root != nullptr) {
            inorder(root->left);
            cout << root->value << " ";
            inorder(root->right);
        }
    }
};

// Main function to demonstrate the tree operations
int main() {
    BinarySearchTree bst;
    Node* root = nullptr;

    // Insert some values into the BST
    int keys[] = {20, 8, 22, 4, 12, 10, 14};
    for (int key : keys) {
        root = bst.insert(root, key);
    }

    cout << "Inorder traversal before deletion: ";
    bst.inorder(root);
    cout << endl;

    // Deleting node with value 10
    root = bst.deleteNode(root, 10);

    cout << "Inorder traversal after deletion of 10: ";
    bst.inorder(root);
    cout << endl;

    return 0;
}


Step 3: Explanation of Cases
In the delete method, we handle three cases:

Node has no children (leaf node): We simply return None to delete it.
Node has one child: We bypass the node by returning its child.
Node has two children: We find the inorder successor (the smallest node in the right subtree), replace the node’s value with the successor’s value, and delete the successor.
