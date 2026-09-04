# Version Control Workflow

## 1. Overview

This document describes the Git version control workflow
used for the MLOps Iris Classifier project.

## 2. Branching Strategy

- main: Stable production-ready code
- develop: Integration and development
- feature/<name>: Individual features
- conflict-demo-*: Conflict resolution practice

## 3. Commit Convention

- feat: New functionality
- fix: Bug fixes
- docs: Documentation changes
- chore: Configuration and maintenance
- refactor: Code restructuring

## 4. Standard Workflow

git switch develop
git pull origin develop
git switch -c feature/<name>
git add <files>
git commit -m "feat: description"
git push -u origin feature/<name>

Then create a Pull Request into develop.

## 5. Conflict Resolution

1. Attempt merge.
2. Open conflicted file.
3. Find conflict markers.
4. Choose or combine changes.
5. Remove conflict markers.
6. Run git add.
7. Run git commit.
8. Test the program.

## 6. Verification

Run:

python src/train.py

and verify that the program prints Accuracy.