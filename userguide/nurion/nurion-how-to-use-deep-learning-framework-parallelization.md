---
description: 슈퍼컴퓨팅인프라센터 2019. 9. 27. 08:20
---

# 누리온 딥러닝 프레임워크 병렬화 사용법

### **가. Tensorflow에서 Horovod 사용법**

다중노드에서 CPU를 활용할 경우 Horovod를 Tensorflow와 연동하여 병렬화가 가능하다. 아래 예시와 같이 Horovod 사용을 위한 코드를 추가해주면 Tensorflow와 연동이 가능하다. Tensorflow 및 Tensorflow에서 활용 가능한 Keras API 모두 Horovod와 연동이 가능하며 우선 Tensorflow에서 Horovod와 연동하는 방법을 소개한다.\
(예시: MNIST Dataset 및 LeNet-5 CNN 구조)



※ Tensorflow에서 Horovod 활용을 위한 자세한 사용법은 Horovod 공식 가이드 참조\
([https://github.com/horovod/horovod#usage](https://github.com/horovod/horovod#usage))



**◦ Tensorflow에서 Horovod 사용을 위한 import 및 메인 함수에서 Horovod 초기화**

| <p>import horovod.tensorflow as hvd</p><p>...</p><p>hvd.init()</p> |
| ------------------------------------------------------------------ |

※ horovod.tensorflow: Horovod를 Tensorflow와 연동하기 위한 모듈

※ Horovod를 사용하기 위하여 초기화한다.



**◦ 메인 함수에서 Horovod 활용을 위한 Dataset 설정**

| <p>(x_train, y_train), (x_test, y_test) = &#x3C;/p></p><p>keras.datasets.mnist.load_data('MNIST-data-%d' % hvd.rank())</p> |
| -------------------------------------------------------------------------------------------------------------------------- |

※ 각 작업별로 접근할 dataset을 설정하기 위하여 Horovod rank에 따라 설정 및 생성한다.



**◦ 메인 함수에서 optimizer에 Horovod 관련 설정 및 broadcast, 학습 진행 수 설정**

| <p>opt = tf.train.AdamOptimizer(0.001 * hvd.size())</p><p>opt = hvd.DistributedOptimizer(opt)</p><p>global_step = tf.train.get_or_create_global_step()</p><p>train_op = opt.minimize(loss, global_step=global_step)</p><p>hooks = [hvd.BroadcastGlobalVariablesHook(0),</p><p>tf.train.StopAtStepHook(last_step=20000 // hvd.size()), ... ]</p> |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

※ Optimizer에 Horovod 관련 설정을 적용하고 각 작업에 broadcast를 활용하여 전달함

※ 각 작업들의 학습과정 step을 Horovod 작업 수에 따라 설정함



**◦ Inter operation 및 Intra operation의 병렬처리 설정**

| <p>config = tf.ConfigProto()</p><p>config.intra_op_parallelism_threads = int(os.environ[‘OMP_NUM_THREADS’])</p><p>config.inter_op_parallelism_threads = 2</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------- |

※ config.intra\_op\_parallelism\_threads: 연산 작업에서 사용할 thread 개수를 설정하는데 사용되며 job script에서 설정한 OMP\_NUM\_THREADS를 불러와서 적용함 (본 예시의 경우 OMP\_NUM\_THREADS를 32로 설정함)

※ config.intra\_op\_parallelism\_threads: TensorFlow의 작업을 동시에 실행하는 thread 개수이며 예시와 같이 2로 설정할 경우 두 개의 작업이 병렬적으로 실행됨



◦ Rank 0 작업에 Checkpoint 설정

| <p>checkpoint_dir = './checkpoints' if hvd.rank() == 0 else None</p><p>...</p><p>with tf.train.MonitoredTrainingSession(checkpoint_dir=checkpoint_dir,</p><p>hooks=hooks,</p><p>config=config) as mon_sess:</p> |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

※ Checkpoint 저장 및 불러오는 작업은 하나의 프로세스에서 수행되어야 하므로 rank 0번에 설정함

### **나. Intel Caffe에서 다중노드 사용법**

Caffe의 다중노드 병렬화는 Horovod에서 공식적으로 지원하지 않으며 Intel에서 KNL에 최적화하여 개발한 Intel Caffe를 활용하여 병렬처리가 가능하다. Intel Caffe의 경우 병렬처리를 위한 작업들이 모두 코드 개발과정에서 적용되어 기존 Caffe에서 개발된 deploy.prototxt, solver.prototxt, train\_val.prototxt들을 그대로 사용할 수 있다.

※ Intel Caffe 활용을 위한 자세한 사용법은 Intel Caffe 공식 가이드 참조\
([https://github.com/intel/caffe/wiki/Multinode-guide](https://github.com/intel/caffe/wiki/Multinode-guide))

딥러닝 개발자에 의해 수정된 Caffe의 코드에 대하여 병렬처리를 수행할 경우 Intel Caffe 소스코드에 해당 부분을 업데이트하여 컴파일 후 실행하여야 한다.

**◦ Intel Caffe 병렬처리 수행 방법 (스크립트 예제)**

| <p>#!/bin/sh</p><p>#PBS -N test</p><p>#PBS -V</p><p>#PBS -l select=4:ncpus=68:mpiprocs=1:ompthreads=68</p><p>#PBS -q normal</p><p>#PBS -l walltime=04:00:00</p><p>#PBS -A caffe</p><p></p><p>cd $PBS_O_WORKDIR</p><p></p><p>module purge</p><p>module load conda/intel_caffe_1.1.5</p><p></p><p>source /apps/applications/miniconda3/envs/intel_caffe/mlsl_2018.3.008/intel64/bin/mlslvars.sh</p><p></p><p>export KMP_AFFINITY=verbose,granularity=fine,compact=1</p><p>export KMP_BLOCKTIME=1</p><p>export KMP_SETTINGS=1</p><p></p><p><mark style="color:blue;">export OMP_NUM_THREADS=60</mark></p><p><mark style="color:blue;">mpirun -PSM2 -prepend-rank caffe train &#x3C;/p></mark></p><p><mark style="color:blue;">--solver ./models/intel_optimized_models/multinode/alexnet_4nodes/solver.prototxt</mark></p><p><mark style="color:blue;"># 혹은</mark></p><p><mark style="color:blue;">./scripts/run_intelcaffe.sh --hostfile $PBS_NODEFILE &#x3C;/p></mark></p><p><mark style="color:blue;">--caffe_bin /apps/applications/miniconda3/envs/intel_caffe/bin/caffe &#x3C;/p></mark></p><p><mark style="color:blue;">--solver models/intel_optimized_models/multinode/alexnet_4nodes/solver.prototxt &#x3C;/p></mark></p><p><mark style="color:blue;">--network opa --ppn 1 --num_omp_threads 60</mark></p><p>exit 0</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

※ Network 옵션: Intel Onmi-Path Architecture (OPA)로 설정

※ PPN: Process per node의 약자로 노드당 작업수를 뜻함 (기본값: 1)

※ Script를 이용하지 않고도 MPI 실행을 통해 기존 Caffe 수행 방법과 동일하게 수행 가능
