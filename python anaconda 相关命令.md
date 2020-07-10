#### python anaconda 相关命令

 1. 更新所有包: conda upgrade --all,列出所安装的包: conda list

 2. 安装包: conda install package_name1(包名称);批量安装: conda install package_name1 package_name2...; 安装时给包添加版本号:例如---->conda install python=3.6

 3.  卸载包: conda remove package_name(包名称)

 4. 更新包:conda update package_name

 5. 安装nb_conda用于notebook自动关联nb_conda的环境

    ​	(1) conda install nb_conda

	6. 在终端中创建环境:

    ​	(1) : conda create -n env_name packages_name

    ​	(2) : env_name 是创建环境名称, packages_name 是安装在创建环境中的包名称.例如: conda create 	-n py3 pandas

    ​	(3) : 创建环境时,可以指定要安装在环境中的python版本

    ​	(4) : 例如: conda create -n py3 python=3.6或conda create -n py2 python=2.7

    ​	(5) : 进入环境命令(windows,在Linux中前加source): activate my_env(我的环境),退出环境命令: 			deactivate my_env

    ​	(6) : 进入环境时, 可以在自己环境中安装相关包,conda install package_name

    ​	(7) : 共享环境:它能让其他人安装你的代码中使用的所有包，并确保这些包的版本正确

    ​			1.输出环境所有包到指定文件: conda env export > environment.yaml  将你当前的环境保存到				文件中保存为yaml文件 (输出环境到指定文件之前,必须要进入当前这个环境,至于输出的yaml				格式文件会自动保存在当前操作的路径下)

    ​			2.导出的环境文件后,在其他电脑上使用方法:首先,在conda 环境中进入你的环境,使用命令:conda env 				update -f=/path/to/environment.yml   (/path/to/environment.yml是需要导入的环境文件路径)

    ​			3.如果不使用conda, 另外一种方式: pip freeze > environment.txt,首先可以在conda环境中使用这个				命令,然后再另外一台电脑更新环境: pip install -r /path/to/environment.txt

    ​	(8) : 列出环境命令: conda env list 	就可以列出你创建的所有环境.

    ​	(9) : 删除环境命令: conda env remove -n env_name(自己创建的环境)

	7. 快速使用jupyter notebook

    ​	(1) : 安装环境自动关联notebook包: conda install nb_conda

    ​	(2) : 如果使用自己创建的环境的话,需要安装ipykernel,具体是进入到自己创建的环境中,输入: pip/conda 		install  ipykernel

    ​	(3) : 最新版anaconda 自带 pyreadline(代码自动补全包),如果没有需要安装: conda install pyreadline

    ​		启动jupyter notebook 时按下 tab 键自动代码,提高效率.

    ​	(4) : anaconda 常用命令:

    ​			









