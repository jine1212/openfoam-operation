*求解器算例2*
---
*openFoam 求解器算例2*


1/  
算例文件：
/home/dyfluid/OKS-toUser/OKS2021March/programming/3.Write/1.icoFoamTemp

复制于openfoam   solvers   incompres  icofoam：


2/  
修改make - files：

exe=   foam_USER_APPBIN    icoFoamTest


3/  
终端: `wmake`   (编译求解器)

4/  
修改icoFoam.c   : 

	fvScalarMatrix sEqn//求解标量s的代码
	{ 
	    fvm::ddt(s)
	  + fvm::div(phi, s)
	  - fvm::laplacian(DT, s) 
	};
	
	sEqn.solve();

5/  
把creatFields.h   上面的volScalarField p    复制一下，
放到最下面  改为s （这一步是为了在.H头文件中添加s场）

6/  
把creatFields.h   上面的dimensionedS-  p    复制一下，
放到最下面  改为DT
删掉dimV-
写dimensionSet (0,2,-1,0,0); //在.H头文件中声明DT标量，设置DT标量的单位