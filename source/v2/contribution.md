---
title: Contribution
---

# Contribution

This document describes how to contribute to the framework.

## Bug report

CatLib strongly encourages the use of GitHub's `pull requests` (PR) to provide bug reports. The bug report can also be submitted via a pull request that contains a failed assertion.

If you submit a bug report as a document, your question should include a title and a clear description of the issue, as well as as much relevant information as possible and a code template to demonstrate the problem. The purpose of the bug report is to get you It's easier for yourself and others to reproduce defects and fix them.

## Discussion

You can suggest new features or optimize existing features on **issue**. If it's new, please implement at least part of the code to complete the new feature development.

## Pull Request

- Don't make PR for larger content unless it's a new component.
- Only one thing per PR
- Make sure the PR code can be compiled
- Please ensure that all tests for the code must pass
- Please be sure to record the reason for PR
- Please ensure unit test coverage for new content
- Please follow the code [style guide] (style.html) to correctly write the code

## PR branch

- All bug fixes should be committed to the latest stable branch.
- Never committed bug fixes to the master branch unless they fix the features that existed in the next release.
- Minor features that are backward compatible can also be committed to the latest stable branch.
- The important new feature is to be committed to the master branch.
- If you are not sure whether it is an important feature or a secondary feature, please consult in **PR**, **calib.slack** or **email**.