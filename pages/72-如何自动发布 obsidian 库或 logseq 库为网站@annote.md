title::  如何自动发布 obsidian 库或 logseq 库为网站

# MetaDate
	- Origin
		- [fishyer.notion.site](https://fishyer.notion.site/obsidian-logseq-ab5ad3d994324cea9f5c909a70653e05#370803ddbaef45e1a5683edefafb2461)
	- Date
		- 2022年08月11日 16:49:28
	- Desc
		- A new tool that blends your everyday work apps into one. It's the all-in-one workspace for you and yo......
	- Tags
		- #Logseq #笔记法
	- Backlinks
		-
	- Reference
		-
# Annoations

collapsed:: false
	- collapsed:: true
	  #+BEGIN_QUOTE
	    *   1-3 - 配置 logseq 的 git 自动提交
	  
	  *   本来 Obsidian 库也有 git 插件自动提交，但是好像没法指定特定文件夹，只能全库提交，这里只提交公开的 logseq 库
	  
	  ![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fe10e907b-753d-4ed3-914d-a68a088cfb24%2FUntitled.png?table=block&id=249d9060-ab04-4235-9f96-04ff6326c8c8&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=2000&userId=&cache=v2)
	  
	  *   1-4 - 配置 git hooks，commit 时自动 push 到 github 仓库
	  
	  *   主要是因为 logseq 的 git，只能自动 commit，不能自动 push，故需添加 git hooks
	  
	  *   路径如下
	  
	  ![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fcebf89a4-5dbd-4736-827b-fb7537c7b6fd%2FUntitled.png?table=block&id=9f6e1f6b-f1c5-4a13-8e3d-13a1e40a50a6&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=2000&userId=&cache=v2)
	  
	  *   shell 脚本文件如下
	  
	  ```
	  #!/bin/sh 
	  git push origin main
	  Shell
	  ``` 
	    #+END_QUOTE
	    collapsed:: true
		- Note
			- #+BEGIN_QUOTE
			   
			  #+END_QUOTE
		- Tags
			-
		- External reference
			-
		- Association
			- #+BEGIN_QUOTE
			  
			  #+END_QUOTE
			- [内部链接](<http://localhost:7026/pdf/如何自动发布 obsidian 库或 logseq 库为网站#id=1660207765743>) |  [外部链接](<>)
			  collapsed:: false
	- collapsed:: true
	  #+BEGIN_QUOTE
	    ![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fe10e907b-753d-4ed3-914d-a68a088cfb24%2FUntitled.png?table=block&id=249d9060-ab04-4235-9f96-04ff6326c8c8&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=2000&userId=&cache=v2) 
	    #+END_QUOTE
	    collapsed:: true
		- Note
			- #+BEGIN_QUOTE
			   
			  #+END_QUOTE
		- Tags
			-
		- External reference
			-
		- Association
			- #+BEGIN_QUOTE
			  
			  #+END_QUOTE
			- [内部链接](<http://localhost:7026/pdf/如何自动发布 obsidian 库或 logseq 库为网站#id=1660207768203>) |  [外部链接](<>)