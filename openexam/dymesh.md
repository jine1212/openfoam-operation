*动态网格算例*
---
*openFoam 动态网格算例*

1/  
算例文件夹：
/home/dyfluid/OKS-toUser/OKS2021March/applications/1_incom/3_dynMesh

2/  
`blockMesh`

3/  
把 文件夹  turbulenceProperties 名称改为 momentumTransport

4/

(1)修改 0-U：

	topand    slip;    
	inlet     fixed; 
	          (2 0 0);   
	outlet    zero;     
	front     empty;   
	cylinder  movingWallVelocity;
	          (0 0 0);
(2)修改 0-P：  

	topand    slip;    
	inlet     zero;   
	outlet    zero;     
	front     empty;   
	cylinder  zero;

(3)修改 constant - dynamicMeshDict  ：  
displacementlaplacian  （对应弹性收缩变形元） 设置为`cylinder`

(4)修改 0 ：

	pointDisplacement：（0 1 0 0 0 0 0）; //米
	 
	topAndBottom     fixed; 
                     (0 0 0);
	      
	cylinder  oscillatingDisplacement; //震动网格

5/  
添加传输方程

controldict  最下面   加 

	functions:
	{
	   #includeFunc   scalarTransport
	}

6/  
修改0 -s ：  
 单位设置 全0  
outlet：zero（只要是流出去的东西都是零法向）  
inlet：fixed   1    
设置头部object 为 s

7/  
(1) 修改system -fvsch：  
divS ：添加 div（phi，s）  Gauss upwind  
若为高阶格式    
divS ：添加 div（phi，s）   Gauss vanLeer


(2) 修改system -fvsolu：  
 添加一个 s { }     
从  含有  
{solver  smoothSolver- }  
 复制过去

8/  
终端`pimpleFoam ` 

