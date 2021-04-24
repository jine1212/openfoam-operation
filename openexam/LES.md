*LES算例*
---
*openFoam LES算例*



1/  
打开算例文件夹 /OKS-toUser/OKS2021March/applications/1_incom/2_turbulence/3_elbow_pisoFoam_test_TS

文件夹内 ctrl+T  打开另一个同级文件夹  

把文件夹    /home/dyfluid/OpenFOAM/OpenFOAM-8/tutorials/incompressible/pisoFoam/LES/pitzDaily   里面 三个文件  复制到算例文件夹

2/  
生成网格：终端： fluentMeshToFoam refine.msh  

> 注意：算例文件夹内打开终端

3/  
修改： constant文件夹中的  momentumTransport  文件 中的  LES - model  ：   Smagorinsky

4/  
边界条件名称查ploymesh文件夹中的boundary  
<font color="red">修改注意严格控制代码中**函数**的空格的形式和数量</font>


修改： 0 文件夹中 的 P：

	INLET1： type zeroGradient; 
	INLET2： type zeroGradient;
	OULET： type  fixedValue;  
			Value  uniform 0;
	wall： type zeroGradien; 
	front： type empty;

修改： 0 文件夹中 的 U：

	INLET1： type  fixedValue;  
			Value  uniform (1 0 0); 
	INLET2： type  fixedValue;  
			Value  uniform (0 3 0); 
	OULET： type zeroGradien;
	wall： type noslip; 
	front： type empty;

修改： 0 文件夹中 的 nut

	INLET1： type  calculatedValue;  
			Value  uniform 0; 
	INLET2： type  calculatedValue;  
			Value  uniform 0; 
	OULET： type  calculatedValue;  
			Value  uniform 0; 
	wall： type nutkWallFunction;   
			Value  uniform 0; 
	front： type empty;

5/  
修改： system  文件夹中 的  cotrolDict :   删除尾部 所有 functions 

6/  
运算调节：


修改system  文件夹中 的  cotrolDict： 

> deltaT ：1e-02  
> 
> endtime：50  
> 

修改constant文件夹中的 transportProperties  
> 
> nu ： 1e-06

7/  
终端输入 pisoFoam > log 

8.后处理

(1)残差：

在第7步 运行之前操作：终端：foamGet residuals  
在 system  文件夹中 的  cotrolDict 添加:

	functions
	{
	       #includeFunc residuals
	}
执行第7步  运行

ctrl+shift+t  调用新终端窗口：foamMonitor -l postP    后面一直tab到 .dat 文件  然后回车查看残差

(2)算法优化：
并行：

把 OpenFOAM/OpenFOAM-8/tutorials/incompressible/pimpleFoam/laminar/mixerVesselAMI2D   的system中的  decomposeParDict 复制到算例的system

修改：decomposeParDict   写上真实的计算机核数  
修改：decomposeParDict   - method  ： scotch

> 例如：
> 
> mpirun -np 60 simpleFoam -parallel 
> 
> mpirun -np 60 simpleFoam -parallel log

强制覆盖  并行计算： decomposePar  -force  
