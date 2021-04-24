*openFOAM 代码文件夹说明*
---
*openFoam 代码文件夹说明*

**文件夹0**  
设置流场参数，比如速度，压力   只与边界条件有关

**文件夹constant**  
- ploymesh：含网格文件即模型文件，含boundary文件，流场参数内的边界条件定义来源于boundary文件  
- momentumTransport：修改流场类型  如laminar / RAS / DNS  
- transportProperties：设置流体特性，如牛顿流体，粘度值nu / mu  

**文件夹system**  
- blockMeshDict：网格要求  
- controlDict：求解过程控制  
- fvSchemes：求解格式设置  
- fvSolution：求解器设置  


**其余文件夹**  
applications：全是程序  
solvers ：求解器，自己写一定要改求解器  
test  ：内部做测试，基本用不到  
utili：小的程序都在这，生成网格之类命令的  
bin：可执行的脚本，帮助快速处理，基本用不到  
etc：openFoam 内部的最基本的要素，基本不要动  
platfoam：编译出来的文件，全是自动生成 的 不用动  
wmake：不用动  
src：open foam的源代码聚集地，可以修改。全是库文件