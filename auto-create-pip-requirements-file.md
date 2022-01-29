---
id: 20220129143003
uuid: 4accf4ae-5a7f-4064-a791-c342c3ddc755
title: Automatically create a Pip requirements.txt
date: 2022-01-29 14:30:03
modified: 
types: tech-note
categories: python
link: https://stackoverflow.com/questions/31684375/automatically-create-requirements-txt
pinned: false
tags: [python, pip, requirements, dependencies]
private: false
draft: false
---

> You can use the following code to generate a requirements.txt file:
> 
>     pip install pipreqs
>     
>     pipreqs /path/to/project
>     
> 
> more info related to pipreqs can be found [here](https://github.com/bndr/pipreqs).
> 
> Sometimes you come across `pip freeze`, but this saves all packages in the environment including those that you don't use in your current project.

Simply run the below in a Python directory and it will create a `requirements.txt` populated with current dependencies.

```sh
pipreqs --force .
```

You can then install all those dependencies by pointing `pip` to the file.

```sh
pip install -r requirements.txt
```
