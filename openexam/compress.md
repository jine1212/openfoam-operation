*可压缩流算例*
---
*openFoam 可压缩流算例 激波*

1/  
算例文件夹
/home/dyfluid/OKS-toUser/OKS2021March/applications/3_com/2bump_rhoSimpleFoam

模板来自

(1)  
 /home/dyfluid/OpenFOAM/OpenFOAM-8/tutorials/compressible/rhoSimpleFoam/squareBend  
/home/dyfluid/OKS-toUser/OKS2021March/applications/3_com/2bump_rhoCentralFoam

(2)  
 /home/dyfluid/OpenFOAM/OpenFOAM-8/tutorials/compressible/rhoCentralFoam/forwardStep

 

2/  
终端 `fluentMeshToFoam  fluent.msh `
（fluent.msh来自于算例文件夹）

3/  
修改ploymesh - boundry:  
 SYMP5   改为 empty

4/  
(1)修改 0 - p:

内部场 101325  
   
	inlet   101325;    
	outlet  zeroGradient;       
	wall4   zeroGradient;                     
	wall3   symmetry;       
	symp5   不管


(2)修改 0 - U:

内部场 (572 0 0)

	inlet   (572 0 0);      
	outlet  zeroGradient;       
	wall4   slip;//（无粘度流） 
	wall3   symmetry;      
	symp5   不管

(3)修改 0 - T:

内部场 300

	inlet   300;            
	outlet  zeroGradient;       
	wall4   zeroGradient;                      
	wall3   symmetry;      
	symp5   不管

如果是超音速则

	outlet  supersonic;

5/  
修改 constant  - thermophysicalProperties :

音速跟摩尔系数有关， 
(perfectGas )   

	molweg-：28.96      
	cp：1004.5     
	mu：0

6/  
库朗特数太大， 就调小步长 deltaT  
修改system - controlDict:

      deltaT:    0.00001
	  endtime:   0.05

7/  
下面两个文件放到system里面，
> refineMeshDict   
> topoSetDict

修改topoSetDict：actions：name c0    
终端：`topoSet`

修改refineMeshDict ： （对c0进行细化）      
directions：（tan1 一个方向  设置三个方向则一个网格变8个网格）   
终端：`refineMesh  -overwrite`

8/  
终端：`rhoCen `-（tab补全）运行