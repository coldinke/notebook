# Git 学习笔记

## Git Cheatsheet

### Basics

- `git help <command>`: get help for a git command
- `git init`: creates a new git repo, with data stored in the `.git` directory
- `git status`: tells you what’s going on
- `git add <filename>`: adds files to staging area
- `git commit`: creates a new commit
    - Write [good commit messages](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)!
    - Even more reasons to write [good commit messages](https://chris.beams.io/posts/git-commit/)!
- `git log`: shows a flattened log of history
- `git log --all --graph --decorate`: visualizes history as a DAG
- `git diff <filename>`: show changes you made relative to the staging area
- `git diff <revision> <filename>`: shows differences in a file between snapshots
- `git checkout <revision>`: updates HEAD and current branch

### Branching and merging

- `git branch`: shows branches
- `git branch <name>`: creates a branch
- `git checkout - b <name>`: creates a branch and switches to it 
	- same as `git branch <name>; git checkout <name>`
- `git merge <revision>`: merges into current branch
- `git mergetool`: use a fancy tool to help resolve merge conflicts
- `git rebase`: rebase set of patches onto a new base

### Remotes

- `git remote`: list remotes
- `git remote add <name> <url>`: add a remote
- `git push <remote> <local branch>:<remote branch>`: send objects to remote, and update remote reference
- `git branch --set-upstream-to=<remote>/<remote branch>`: set up correspondence between local and remote branch
- `git fetch`: retrieve objects/references from a remote
- `git pull`: same as `git fetch; git merge`
- `git clone`: download repository from remote

### Undo

- `git commit --amend`: edit commit's contents/message
- `git reset HEAD <file>`: unstage a file
- `git checkout -- <file>`: discard changes

### Advanced Git 

- `git config`: Git is  [highly customizable](https://git-scm.com/docs/git-config)
- `git clone --depth=1`: shallow clone, without entire version history 
- `git add -p`: interactive staging
- `git rebase -i`: interactive rebasing
- `git blame`: show who last which line
- `git stash`: temporarily remove modifications to working directory
- `git biset`: binary search history(e.g. for regressions)
- `.gitignore`: [specify](https://git-scm.com/docs/gitignore) intentionally untracked files to ignore


## What is Git or VCSs (Version control systems) ?

> Version control systems (VCSs) are tools used to track changes to source code (or other collections of files and folders).
> 		-- missing-semester


### Git's data model

#### Snapshots

In Git terminology, a file is called a **"blob"**, and it's just a bunch of bytes. A directory is called a **"tree"**, and it's maps names to blobs or trees (so directories can contain other directories). A snapshot is the top-level tree is being tracked. 

Example:
```bash
<root> (tree)
|
+- foo (tree)
|  |
|  + bar.txt (blob, contents = "hello world")
|
+- baz.txt (blob, contents = "git is wonderful")
```

The top-level tree contains two elements,  a tree "foo" (that itself contains one element, a blob "bar.txt"), and a blob "baz.txt"


### Modeling history: relating snapshots

How should a version control system relate snapshots? 

One simple model would be to have a linear history. A history would be a list of snapshots in time-order. 

In Git, a history is a directed acyclic graph (DAG) of snapshots.  All this means is that each snapshot in Git refers to a set of "parents", the snapshots that preceded it. It's set of parent rather than single parent (as would be the case in a linear history) because a snapshot might descend from multiple parents, for example, due to combining (merging) two parallel branches of development. 

Git calls these snapshots "commit"s. Visualizing a commit history might look something like this:

```bash
o <-- o <-- o <-- o
            ^
             \
              --- o <-- o
```

In the ASCII art above, the `o`s correspond to individual commits (snapshots). The arrows point to the parent of each commit (it's a "comes before" relation, not "comes after"). After third commit, the history branches into two separate branches. This might correspond to, for example, two separate features being developed in parallel, independently from each other. In the future, these branches may be merged to create a new snapshot that incorporates both of the features, producing a new history that looks like this, with newly created merge commit shown in bold:

```

o <-- o <-- o <-- o <---- o
            ^            /
             \          v
              --- o <-- o
```

Commits in Git are **immutable**. This doesn't mean that mistakes can't be corrected, however; it's just that "edits" to the commit history are **actually creating entirely new commits**, and references are updated to point to the new ones.

### Date model, as pseudocode

It may be instructive to see Git's data model written down in pseudocode:

```
// a file is a bunch of bytes
type blob = array<byte>

// a directory contains named files and directories
type tree = map<string, tree | blob>

// a commit has parents, metdata, and the top-level tree
type commit = struct {
	parents: array<commit>
	author: string
	message: string
	snapshot: tree
}
```

### Objects and content-addressing

An "object" is a blob, tree, or commit:

`type object = blob | tree | commit`

In Git data store, all object are content-addressed by their [SHA-1 hash](https://en.wikipedia.org/wiki/SHA-1)

```python
objects = map<string, object>

def store(object):
	id = sha1(object)
	objects[id] = object

def load(id):
	return objects[id]
```

Blobs, trees, and commits are unified in this way: they are all objects. When they reference other objects, they don't actually *contain* them in their on-disk representation, but have a reference to them by their hash.

For example, the tree for the example directory structure [above](https://missing.csail.mit.edu/2020/version-control/#snapshots) (visualized using `git cat-file -p 698281bc680d1995c5f4caaf3359721a5a58d48d`), looks like this:
```bash
100644 blob 4448adbf7ecd394f42ae135bbeed9676e894af85    baz.txt
040000 tree c68d233a33c5c06e0340e4c224f0afca87c8ce87    foo
```

The tree itself contains pointers to its contents, `baz.txt` (a blob) and `foo` (a tree). If we look at the contents addressed by the hash corresponding to baz.txt with `git cat-file -p HASHCODE`


### References

Because humans are't good at remembering strings of 40 hexadecimal characters.

Git's solution to this problem is human-readable names for SHA-1 hashes, called "references". References are pointers to commits, Unlike objects, which are immutable, references are mutable (can be updated to point to a new commit). For example, the `master` reference usually points to the latest commit in the main branch of development.

```
references = map<string, string>

def update_reference(name, id):
	references[name] = id

def read_reference(name):
	return references[name]

def load_reference(name_or_id):
	if name_or_id in references:
		return load(references[name_or_id])
	else:
		return load(name_or_id)
```

With this, Git can use human-readable names like "master" to refer to a particular snapshot in the history, instead of a long hexadecimal string.

One detail is that we often want a notion of "where we currently are" in the history, so that when we take a new snapshot, we know what it is relative to (how we set the `parents` field of the commit). In Git, that "where we currently are" is a special reference called "HEAD".

### Staging area

This is another concept that's orthogonal to the data model, but it's a part of the interface to create commits.

One way you might imagine implementing snapshotting as described above is to have a "create snapshot" command that creates a new snapshot based on the *current* state of the working directory. Some version control tools work like this, but not Git. We want clean snapshots, and it might not always be ideal to make a snapshot from the current state.

Git accommodates such scenarios by allowing you to specify which modifications should be included in the next snapshot through a mechanism called the "*staging area*".


## More content ...


## Reference 

https://missing.csail.mit.edu/2020/version-control/