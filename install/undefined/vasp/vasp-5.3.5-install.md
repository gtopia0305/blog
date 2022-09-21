---
description: 슈퍼컴퓨팅인프라센터 2018. 12. 5. 18:24
---

# VASP 5.3.5 설치

KISTI 슈퍼컴퓨팅센터의 누리온 시스템에 vasp 5.3.5 Source 버전으로 설치 하는 방법에 대하여 소개 한다.



## **1. 설치 환경**

|  **구분**     | **내용**                      |
| ----------- | --------------------------- |
|  대상 시스템     |  누리온                        |
| OS Version  |  리눅스 / CentOS 7.3           |
|  CPU        |  Intel(R) Xeon(R) Gold 6148 |
|  컴파일러       |  Intel 2018.3 Version       |
|  MPI        |  IntelMPI 2018.3 Version    |
|  기타         |  Intel MKL Math Library     |



## **2. 설치 전 환경 설정**

&#x20; 누리온 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여 \
&#x20; 환경설정 툴인 Modules(http://modules.sourceforge.net)이 구성되어 있고,\
&#x20; 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.



\[ 환경 설정 ]

> &#x20;$ module load intel/18.0.3 impi/18.0.3

## **3. vasp 5.3.5 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다. &#x20;

|  **설치 과정**                                                                                                                                                                                                                                                                                                                                                                                                             |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p> $ tar xvf vasp.5.lib.tar </p><p> $ tar xvf vasp.5.3.5.tar.gz </p><p> $ cd vasp.5.lib</p><p> $ cp makefile.linux_ifc_P4 makefile</p><p> $ vi makefile</p><p> - - - - - <strong>[vasp.5.lib makefile 파일 수정 내용]</strong> 참고</p><p> $ make</p><p> $ cd ../vasp.5.3</p><p> $ cp makefile.linux_ifc_P4 makefile</p><p> $ vi makefile</p><p> - - - - - <strong>[vasp.5.3 makefile 파일 수정 내용]</strong> 참고</p><p> $ make</p> |



**\[vasp.5.lib makefile 파일 수정 내용]**

> .SUFFIXES: .inc .f .F
>
>
>
> \# C-preprocessor
>
> CPP     = gcc -E -P -C $\*.F >$\*.f
>
> FC=ifort
>
>
>
> CFLAGS = -O
>
> FFLAGS = -O0 -FI
>
> FREE   =  -FR
>
>
>
> DOBJ =  preclib.o timing\_.o derrf\_.o dclock\_.o  diolib.o dlexlib.o drdatab.o
>
>
>
> \#-----------------------------------------------------------------------
>
> \# general rules
>
> \#-----------------------------------------------------------------------
>
>
>
> libdmy.a: $(DOBJ) lapack\_double.o linpack\_double.o lapack\_atlas.o
>
> &#x20;       \-rm libdmy.a
>
> &#x20;       ar vq libdmy.a $(DOBJ)
>
>
>
> \# files which do not require autodouble&#x20;
>
> lapack\_min.o: lapack\_min.f
>
> &#x20;       $(FC) $(FFLAGS) $(NOFREE) -c lapack\_min.f
>
> lapack\_double.o: lapack\_double.f
>
> &#x20;       $(FC) $(FFLAGS) $(NOFREE) -c lapack\_double.f
>
> lapack\_single.o: lapack\_single.f
>
> &#x20;       $(FC) $(FFLAGS) $(NOFREE) -c lapack\_single.f
>
> lapack\_atlas.o: lapack\_atlas.f
>
> &#x20;       $(FC) $(FFLAGS) $(NOFREE) -c lapack\_atlas.f
>
> linpack\_double.o: linpack\_double.f
>
> &#x20;       $(FC) $(FFLAGS) $(NOFREE) -c linpack\_double.f
>
> linpack\_single.o: linpack\_single.f
>
> &#x20;       $(FC) $(FFLAGS) $(NOFREE) -c linpack\_single.f
>
>
>
> .c.o:
>
> &#x20;       $(CC) $(CFLAGS) -c $\*.c
>
> .F.o:
>
> &#x20;       $(CPP)&#x20;
>
> &#x20;       $(FC) $(FFLAGS) $(FREE) $(INCS) -c $\*.f
>
> .F.f:
>
> &#x20;       $(CPP)&#x20;
>
> .f.o:
>
> &#x20;       $(FC) $(FFLAGS) $(FREE) $(INCS) -c $\*.f



**\[vasp.5.3 makefile 파일 수정 내용]**

> .SUFFIXES: .inc .f .f90 .F
>
> SUFFIX=.f90
>
>
>
> FC=mpiifort
>
> FCL=$(FC)
>
>
>
> CPP\_=fpp -f\_com=no -free -w0 $\*.F $\*$(SUFFIX)
>
> \
> FFLAGS =  -FR -assume byterecl
>
>
>
> OFLAG=-O3 -ip -ftz -xCORE-AVX2
>
> OFLAG\_HIGH = $(OFLAG)
>
> OBJ\_HIGH =
>
> OBJ\_NOOPT =
>
> DEBUG  = -FR -O0
>
> INLINE = $(OFLAG)
>
>
>
> MKL\_PATH=$(MKLROOT)/lib/intel64
>
>
>
> BLAS= -mkl=cluster
>
>
>
> BLACS =&#x20;
>
> SCALAPACK =&#x20;
>
> \
> CPP= $(CPP\_) -DMPI -DHOST=\\"LinuxIFC\\" -DIFC \\
>
> &#x20;\-DCACHE\_SIZE=16000 -DPGF90 -Davoidalloc -DNGZhalf \\
>
> &#x20;\-DMPI\_BLOCK=8000 -Duse\_collective \\
>
> &#x20;\-DRPROMU\_DGEMV -DRACCMU\_DGEMV
>
>
>
> LIB  = -L../vasp.5.lib -ldmy \\
>
> &#x20;    ../vasp.5.lib/linpack\_double.o \\
>
> &#x20;    $(BLAS) $(SCALAPACK)
>
>
>
> LINK = -shared-intel
>
>
>
> INCS= -I$(MKLROOT)/include/fftw
>
> FFT3D= fftmpiw.o fftmpi\_map.o fftw3d.o fft3dlib.o \\
>
> &#x20;      $(MKLROOT)/lib/intel64/libfftw3xf\_intel.a
>
>
>
> BASIC=   symmetry.o symlib.o   lattlib.o  random.o
>
>
>
> SOURCE=  base.o     mpi.o      smart\_allocate.o      xml.o  \\
>
> &#x20;        constant.o jacobi.o   main\_mpi.o  scala.o   \\
>
> &#x20;        asa.o      lattice.o  poscar.o   ini.o  mgrid.o  xclib.o  vdw\_nl.o  xclib\_grad.o \\
>
> &#x20;        radial.o   pseudo.o   gridq.o     ebs.o  \\
>
> &#x20;        mkpoints.o wave.o     wave\_mpi.o  wave\_high.o  spinsym.o \\
>
> &#x20;        $(BASIC)   nonl.o     nonlr.o    nonl\_high.o dfast.o    choleski2.o \\
>
> &#x20;        mix.o      hamil.o    xcgrad.o   xcspin.o    potex1.o   potex2.o  \\
>
> &#x20;        constrmag.o cl\_shift.o relativistic.o LDApU.o \\
>
> &#x20;        paw\_base.o metagga.o  egrad.o    pawsym.o   pawfock.o  pawlhf.o   rhfatm.o  hyperfine.o paw.o   \\
>
> &#x20;        mkpoints\_full.o       charge.o   Lebedev-Laikov.o  stockholder.o dipol.o    pot.o \\
>
> &#x20;        dos.o      elf.o      tet.o      tetweight.o hamil\_rot.o \\
>
> &#x20;        chain.o    dyna.o     k-proj.o    sphpro.o    us.o  core\_rel.o \\
>
> &#x20;        aedens.o   wavpre.o   wavpre\_noio.o broyden.o \\
>
> &#x20;        dynbr.o    hamil\_high.o  rmm-diis.o reader.o   writer.o   tutor.o xml\_writer.o \\
>
> &#x20;        brent.o    stufak.o   fileio.o   opergrid.o stepver.o  \\
>
> &#x20;        chgloc.o   fast\_aug.o fock\_multipole.o  fock.o  mkpoints\_change.o sym\_grad.o \\
>
> &#x20;        mymath.o   internals.o npt\_dynamics.o   dynconstr.o dimer\_heyden.o dvvtrajectory.o subdftd3.o \\
>
> &#x20;        vdwforcefield.o nmr.o      pead.o     subrot.o   subrot\_scf.o  paircorrection.o \\
>
> &#x20;        force.o    pwlhf.o    gw\_model.o optreal.o  steep.o    davidson.o  david\_inner.o \\
>
> &#x20;        electron.o rot.o  electron\_all.o shm.o    pardens.o  \\
>
> &#x20;        optics.o   constr\_cell\_relax.o   stm.o    finite\_diff.o elpol.o    \\
>
> &#x20;        hamil\_lr.o rmm-diis\_lr.o  subrot\_cluster.o subrot\_lr.o \\
>
> &#x20;        lr\_helper.o hamil\_lrf.o   elinear\_response.o ilinear\_response.o \\
>
> &#x20;        linear\_optics.o \\
>
> &#x20;        setlocalpp.o  wannier.o electron\_OEP.o electron\_lhf.o twoelectron4o.o \\
>
> &#x20;        gauss\_quad.o m\_unirnk.o minimax\_tabs.o minimax.o \\
>
> &#x20;        mlwf.o     ratpol.o screened\_2e.o wave\_cacher.o chi\_base.o wpot.o \\
>
> &#x20;        local\_field.o ump2.o ump2kpar.o fcidump.o ump2no.o \\
>
> &#x20;        bse\_te.o bse.o acfdt.o chi.o sydmat.o \\
>
> &#x20;        lcao\_bare.o wnpr.o dmft.o \\
>
> &#x20;        rmm-diis\_mlr.o  linear\_response\_NMR.o wannier\_interpol.o linear\_response.o  auger.o getshmem.o \\
>
> &#x20;        dmatrix.o
>
>
>
> vasp: $(SOURCE) $(FFT3D) $(INC) main.o
>
> &#x20;       rm -f vasp
>
> &#x20;       $(FCL) -o vasp main.o  $(SOURCE)   $(FFT3D) $(LIB) $(LINK)
>
> makeparam: $(SOURCE) $(FFT3D) makeparam.o main.F $(INC)
>
> &#x20;       $(FCL) -o makeparam  $(LINK) makeparam.o $(SOURCE) $(FFT3D) $(LIB)
>
> zgemmtest: zgemmtest.o base.o random.o $(INC)
>
> &#x20;       $(FCL) -o zgemmtest $(LINK) zgemmtest.o random.o base.o $(LIB)
>
> dgemmtest: dgemmtest.o base.o random.o $(INC)
>
> &#x20;       $(FCL) -o dgemmtest $(LINK) dgemmtest.o random.o base.o $(LIB)&#x20;
>
> ffttest: base.o smart\_allocate.o mpi.o mgrid.o random.o ffttest.o $(FFT3D) $(INC)
>
> &#x20;       $(FCL) -o ffttest $(LINK) ffttest.o mpi.o mgrid.o random.o smart\_allocate.o base.o $(FFT3D) $(LIB)
>
> kpoints: $(SOURCE) $(FFT3D) makekpoints.o main.F $(INC)
>
> &#x20;       $(FCL) -o kpoints $(LINK) makekpoints.o $(SOURCE) $(FFT3D) $(LIB)
>
>
>
> clean: &#x20;
>
> &#x20;       \-rm -f \*.g \*.f90 \*.o \*.L \*.mod ; touch \*.F
>
>
>
> main.o: main$(SUFFIX)
>
> &#x20;       $(FC) $(FFLAGS) $(DEBUG)  $(INCS) -c main$(SUFFIX)
>
> xcgrad.o: xcgrad$(SUFFIX)
>
> &#x20;       $(FC) $(FFLAGS) $(INLINE)  $(INCS) -c xcgrad$(SUFFIX)
>
> xcspin.o: xcspin$(SUFFIX)
>
> &#x20;       $(FC) $(FFLAGS) $(INLINE)  $(INCS) -c xcspin$(SUFFIX)
>
>
>
> makeparam.o: makeparam$(SUFFIX)
>
> &#x20;       $(FC) $(FFLAGS) $(DEBUG)  $(INCS) -c makeparam$(SUFFIX)
>
>
>
> makeparam$(SUFFIX): makeparam.F main.F
>
>
>
> base.o: base.inc base.F
>
> mgrid.o: mgrid.inc mgrid.F
>
> constant.o: constant.inc constant.F
>
> lattice.o: lattice.inc lattice.F
>
> setex.o: setexm.inc setex.F
>
> pseudo.o: pseudo.inc pseudo.F
>
> mkpoints.o: mkpoints.inc mkpoints.F
>
> wave.o: wave.F
>
> nonl.o: nonl.inc nonl.F
>
> nonlr.o: nonlr.inc nonlr.F
>
> \
> $(OBJ\_HIGH):
>
> &#x20;       $(CPP)
>
> &#x20;       $(FC) $(FFLAGS) $(OFLAG\_HIGH) $(INCS) -c $\*$(SUFFIX)
>
> $(OBJ\_NOOPT):
>
> &#x20;       $(CPP)
>
> &#x20;       $(FC) $(FFLAGS) $(INCS) -c $\*$(SUFFIX)
>
> \
> fft3dlib\_f77.o: fft3dlib\_f77.F
>
> &#x20;       $(CPP)
>
> &#x20;       $(F77) $(FFLAGS\_F77) -c $\*$(SUFFIX)
>
> \
> .F.o:
>
> &#x20;       $(CPP)
>
> &#x20;       $(FC) $(FFLAGS) $(OFLAG) $(INCS) -c $\*$(SUFFIX)
>
> .F$(SUFFIX):
>
> &#x20;       $(CPP)
>
> $(SUFFIX).o:
>
> &#x20;       $(FC) $(FFLAGS) $(OFLAG) $(INCS) -c $\*$(SUFFIX)
>
> \
> \# special rules
>
> \#-----------------------------------------------------------------------
>
> \# these special rules have been tested for ifc.11 and ifc.12 only
>
> \
> fft3dlib.o : fft3dlib.F
>
> &#x20;       $(CPP)
>
> &#x20;       $(FC) -FR -lowercase -O2 -c $\*$(SUFFIX)
>
> fft3dfurth.o : fft3dfurth.F
>
> &#x20;       $(CPP)
>
> &#x20;       $(FC) -FR -lowercase -O1 -c $\*$(SUFFIX)
>
> fftw3d.o : fftw3d.F
>
> &#x20;       $(CPP)
>
> &#x20;       $(FC) -FR -lowercase -O1 $(INCS) -c $\*$(SUFFIX)
>
> fftmpi.o : fftmpi.F
>
> &#x20;       $(CPP)
>
> &#x20;       $(FC) -FR -lowercase -O1 -c $\*$(SUFFIX)
>
> fftmpiw.o : fftmpiw.F
>
> &#x20;       $(CPP)
>
> &#x20;       $(FC) -FR -lowercase -O1 $(INCS) -c $\*$(SUFFIX)
>
> wave\_high.o : wave\_high.F
>
> &#x20;       $(CPP)
>
> &#x20;       $(FC) -FR -lowercase -O1 -c $\*$(SUFFIX)
>
> \# the following rules are probably no longer required (-O3 seems to work)
>
> wave.o : wave.F
>
> &#x20;       $(CPP)
>
> &#x20;       $(FC) -FR -lowercase -O2 -c $\*$(SUFFIX)
>
> paw.o : paw.F
>
> &#x20;       $(CPP)
>
> &#x20;       $(FC) -FR -lowercase -O1 -c $\*$(SUFFIX)
>
> cl\_shift.o : cl\_shift.F
>
> &#x20;       $(CPP)
>
> &#x20;       $(FC) -FR -lowercase -O2 -c $\*$(SUFFIX)
>
> us.o : us.F
>
> &#x20;       $(CPP)
>
> &#x20;       $(FC) -FR -lowercase -O1 -c $\*$(SUFFIX)
>
> LDApU.o : LDApU.F
>
> &#x20;       $(CPP)
>
> &#x20;       $(FC) -FR -lowercase -O2 -c $\*$(SUFFIX)
