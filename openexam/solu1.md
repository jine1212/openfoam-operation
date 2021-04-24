*求解器算例1*
---
*openFoam 求解器算例1*

1/  
算例文件：
/home/dyfluid/OKS-toUser/OKS2021March/programming/3.Write/2.projectionFoam

复制于solvers   incompres  icofoam：

2/  
修改 make - files：  
FOAM_USER_APPBIN   /projectionFoam

3/  
修改icoFoam.c:  

58行开始，至113行 删掉   添加：

	fvVectorMatrix UEqn//求解矢量U的代码
	{
	   fvm::ddt(U) 
	 + fvc::div(phi, U)
	 - fvc::laplacian(nu, U)
	};

	UEqn.solve();//求得U^*

	dimensionedScalar deltaT = runTime.deltaT();//定义Δt
	
	phi = fvc::interpolate(U) & mesh.Sf();//因为openFoam不直接计算div（U），所以要构建通量phi, 求phi 必须插值，其为面上的量。
	
	fvVectorMatrix pEqn
	{
	   fvm::laplacian(P) == fvc::div(phi) /deltaT  //加和通量phi
	};

	pEqn.solve();//求得p^(n+1)
	
	U = - deltaT*fvc::grad(p) + U;   //U^(n+1)

4/  
终端：`wmake`
