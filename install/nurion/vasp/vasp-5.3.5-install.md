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

{% code title="[ 환경 설정 ]" %}
```
 $ module load intel/18.0.3 impi/18.0.3
```
{% endcode %}

## **3. vasp 5.3.5 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다. &#x20;

{% code title=" 설치 과정" %}
```
 $ tar xvf vasp.5.lib.tar 
 $ tar xvf vasp.5.3.5.tar.gz 
 $ cd vasp.5.lib
 $ cp makefile.linux_ifc_P4 makefile
 $ vi makefile
 - - - - - [vasp.5.lib makefile 파일 수정 내용] 참고
 $ make
 $ cd ../vasp.5.3
 $ cp makefile.linux_ifc_P4 makefile
 $ vi makefile
 - - - - - [vasp.5.3 makefile 파일 수정 내용] 참고
 $ make
```
{% endcode %}



{% code title="[vasp.5.lib makefile 파일 수정 내용]" %}
```
.SUFFIXES: .inc .f .F

# C-preprocessor
CPP     = gcc -E -P -C $*.F >$*.f
FC=ifort

CFLAGS = -O
FFLAGS = -O0 -FI
FREE   =  -FR

DOBJ =  preclib.o timing_.o derrf_.o dclock_.o  diolib.o dlexlib.o drdatab.o

#-----------------------------------------------------------------------
# general rules
#-----------------------------------------------------------------------

libdmy.a: $(DOBJ) lapack_double.o linpack_double.o lapack_atlas.o
        -rm libdmy.a
        ar vq libdmy.a $(DOBJ)

# files which do not require autodouble 
lapack_min.o: lapack_min.f
        $(FC) $(FFLAGS) $(NOFREE) -c lapack_min.f
lapack_double.o: lapack_double.f
        $(FC) $(FFLAGS) $(NOFREE) -c lapack_double.f
lapack_single.o: lapack_single.f
        $(FC) $(FFLAGS) $(NOFREE) -c lapack_single.f
lapack_atlas.o: lapack_atlas.f
        $(FC) $(FFLAGS) $(NOFREE) -c lapack_atlas.f
linpack_double.o: linpack_double.f
        $(FC) $(FFLAGS) $(NOFREE) -c linpack_double.f
linpack_single.o: linpack_single.f
        $(FC) $(FFLAGS) $(NOFREE) -c linpack_single.f

.c.o:
        $(CC) $(CFLAGS) -c $*.c
.F.o:
        $(CPP) 
        $(FC) $(FFLAGS) $(FREE) $(INCS) -c $*.f
.F.f:
        $(CPP) 
.f.o:
        $(FC) $(FFLAGS) $(FREE) $(INCS) -c $*.f
```
{% endcode %}



{% code title="[vasp.5.3 makefile 파일 수정 내용]" %}
```
.SUFFIXES: .inc .f .f90 .F
SUFFIX=.f90

FC=mpiifort
FCL=$(FC)

CPP_=fpp -f_com=no -free -w0 $*.F $*$(SUFFIX)

FFLAGS =  -FR -assume byterecl

OFLAG=-O3 -ip -ftz -xCORE-AVX2
OFLAG_HIGH = $(OFLAG)
OBJ_HIGH =
OBJ_NOOPT =
DEBUG  = -FR -O0
INLINE = $(OFLAG)

MKL_PATH=$(MKLROOT)/lib/intel64

BLAS= -mkl=cluster

BLACS = 
SCALAPACK = 

CPP= $(CPP_) -DMPI -DHOST=\"LinuxIFC\" -DIFC \
 -DCACHE_SIZE=16000 -DPGF90 -Davoidalloc -DNGZhalf \
 -DMPI_BLOCK=8000 -Duse_collective \
 -DRPROMU_DGEMV -DRACCMU_DGEMV

LIB  = -L../vasp.5.lib -ldmy \
     ../vasp.5.lib/linpack_double.o \
     $(BLAS) $(SCALAPACK)

LINK = -shared-intel

INCS= -I$(MKLROOT)/include/fftw
FFT3D= fftmpiw.o fftmpi_map.o fftw3d.o fft3dlib.o \
       $(MKLROOT)/lib/intel64/libfftw3xf_intel.a

BASIC=   symmetry.o symlib.o   lattlib.o  random.o

SOURCE=  base.o     mpi.o      smart_allocate.o      xml.o  \
         constant.o jacobi.o   main_mpi.o  scala.o   \
         asa.o      lattice.o  poscar.o   ini.o  mgrid.o  xclib.o  vdw_nl.o  xclib_grad.o \
         radial.o   pseudo.o   gridq.o     ebs.o  \
         mkpoints.o wave.o     wave_mpi.o  wave_high.o  spinsym.o \
         $(BASIC)   nonl.o     nonlr.o    nonl_high.o dfast.o    choleski2.o \
         mix.o      hamil.o    xcgrad.o   xcspin.o    potex1.o   potex2.o  \
         constrmag.o cl_shift.o relativistic.o LDApU.o \
         paw_base.o metagga.o  egrad.o    pawsym.o   pawfock.o  pawlhf.o   rhfatm.o  hyperfine.o paw.o   \
         mkpoints_full.o       charge.o   Lebedev-Laikov.o  stockholder.o dipol.o    pot.o \
         dos.o      elf.o      tet.o      tetweight.o hamil_rot.o \
         chain.o    dyna.o     k-proj.o    sphpro.o    us.o  core_rel.o \
         aedens.o   wavpre.o   wavpre_noio.o broyden.o \
         dynbr.o    hamil_high.o  rmm-diis.o reader.o   writer.o   tutor.o xml_writer.o \
         brent.o    stufak.o   fileio.o   opergrid.o stepver.o  \
         chgloc.o   fast_aug.o fock_multipole.o  fock.o  mkpoints_change.o sym_grad.o \
         mymath.o   internals.o npt_dynamics.o   dynconstr.o dimer_heyden.o dvvtrajectory.o subdftd3.o \
         vdwforcefield.o nmr.o      pead.o     subrot.o   subrot_scf.o  paircorrection.o \
         force.o    pwlhf.o    gw_model.o optreal.o  steep.o    davidson.o  david_inner.o \
         electron.o rot.o  electron_all.o shm.o    pardens.o  \
         optics.o   constr_cell_relax.o   stm.o    finite_diff.o elpol.o    \
         hamil_lr.o rmm-diis_lr.o  subrot_cluster.o subrot_lr.o \
         lr_helper.o hamil_lrf.o   elinear_response.o ilinear_response.o \
         linear_optics.o \
         setlocalpp.o  wannier.o electron_OEP.o electron_lhf.o twoelectron4o.o \
         gauss_quad.o m_unirnk.o minimax_tabs.o minimax.o \
         mlwf.o     ratpol.o screened_2e.o wave_cacher.o chi_base.o wpot.o \
         local_field.o ump2.o ump2kpar.o fcidump.o ump2no.o \
         bse_te.o bse.o acfdt.o chi.o sydmat.o \
         lcao_bare.o wnpr.o dmft.o \
         rmm-diis_mlr.o  linear_response_NMR.o wannier_interpol.o linear_response.o  auger.o getshmem.o \
         dmatrix.o

vasp: $(SOURCE) $(FFT3D) $(INC) main.o
        rm -f vasp
        $(FCL) -o vasp main.o  $(SOURCE)   $(FFT3D) $(LIB) $(LINK)
makeparam: $(SOURCE) $(FFT3D) makeparam.o main.F $(INC)
        $(FCL) -o makeparam  $(LINK) makeparam.o $(SOURCE) $(FFT3D) $(LIB)
zgemmtest: zgemmtest.o base.o random.o $(INC)
        $(FCL) -o zgemmtest $(LINK) zgemmtest.o random.o base.o $(LIB)
dgemmtest: dgemmtest.o base.o random.o $(INC)
        $(FCL) -o dgemmtest $(LINK) dgemmtest.o random.o base.o $(LIB) 
ffttest: base.o smart_allocate.o mpi.o mgrid.o random.o ffttest.o $(FFT3D) $(INC)
        $(FCL) -o ffttest $(LINK) ffttest.o mpi.o mgrid.o random.o smart_allocate.o base.o $(FFT3D) $(LIB)
kpoints: $(SOURCE) $(FFT3D) makekpoints.o main.F $(INC)
        $(FCL) -o kpoints $(LINK) makekpoints.o $(SOURCE) $(FFT3D) $(LIB)

clean:  
        -rm -f *.g *.f90 *.o *.L *.mod ; touch *.F

main.o: main$(SUFFIX)
        $(FC) $(FFLAGS) $(DEBUG)  $(INCS) -c main$(SUFFIX)
xcgrad.o: xcgrad$(SUFFIX)
        $(FC) $(FFLAGS) $(INLINE)  $(INCS) -c xcgrad$(SUFFIX)
xcspin.o: xcspin$(SUFFIX)
        $(FC) $(FFLAGS) $(INLINE)  $(INCS) -c xcspin$(SUFFIX)

makeparam.o: makeparam$(SUFFIX)
        $(FC) $(FFLAGS) $(DEBUG)  $(INCS) -c makeparam$(SUFFIX)

makeparam$(SUFFIX): makeparam.F main.F

base.o: base.inc base.F
mgrid.o: mgrid.inc mgrid.F
constant.o: constant.inc constant.F
lattice.o: lattice.inc lattice.F
setex.o: setexm.inc setex.F
pseudo.o: pseudo.inc pseudo.F
mkpoints.o: mkpoints.inc mkpoints.F
wave.o: wave.F
nonl.o: nonl.inc nonl.F
nonlr.o: nonlr.inc nonlr.F

$(OBJ_HIGH):
        $(CPP)
        $(FC) $(FFLAGS) $(OFLAG_HIGH) $(INCS) -c $*$(SUFFIX)
$(OBJ_NOOPT):
        $(CPP)
        $(FC) $(FFLAGS) $(INCS) -c $*$(SUFFIX)

fft3dlib_f77.o: fft3dlib_f77.F
        $(CPP)
        $(F77) $(FFLAGS_F77) -c $*$(SUFFIX)

.F.o:
        $(CPP)
        $(FC) $(FFLAGS) $(OFLAG) $(INCS) -c $*$(SUFFIX)
.F$(SUFFIX):
        $(CPP)
$(SUFFIX).o:
        $(FC) $(FFLAGS) $(OFLAG) $(INCS) -c $*$(SUFFIX)

# special rules
#-----------------------------------------------------------------------
# these special rules have been tested for ifc.11 and ifc.12 only

fft3dlib.o : fft3dlib.F
        $(CPP)
        $(FC) -FR -lowercase -O2 -c $*$(SUFFIX)
fft3dfurth.o : fft3dfurth.F
        $(CPP)
        $(FC) -FR -lowercase -O1 -c $*$(SUFFIX)
fftw3d.o : fftw3d.F
        $(CPP)
        $(FC) -FR -lowercase -O1 $(INCS) -c $*$(SUFFIX)
fftmpi.o : fftmpi.F
        $(CPP)
        $(FC) -FR -lowercase -O1 -c $*$(SUFFIX)
fftmpiw.o : fftmpiw.F
        $(CPP)
        $(FC) -FR -lowercase -O1 $(INCS) -c $*$(SUFFIX)
wave_high.o : wave_high.F
        $(CPP)
        $(FC) -FR -lowercase -O1 -c $*$(SUFFIX)
# the following rules are probably no longer required (-O3 seems to work)
wave.o : wave.F
        $(CPP)
        $(FC) -FR -lowercase -O2 -c $*$(SUFFIX)
paw.o : paw.F
        $(CPP)
        $(FC) -FR -lowercase -O1 -c $*$(SUFFIX)
cl_shift.o : cl_shift.F
        $(CPP)
        $(FC) -FR -lowercase -O2 -c $*$(SUFFIX)
us.o : us.F
        $(CPP)
        $(FC) -FR -lowercase -O1 -c $*$(SUFFIX)
LDApU.o : LDApU.F
        $(CPP)
        $(FC) -FR -lowercase -O2 -c $*$(SUFFIX)
```
{% endcode %}
