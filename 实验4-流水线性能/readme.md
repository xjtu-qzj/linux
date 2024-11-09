## 实验4 流水线性能实验

### 实验设置：

**将gem-assighment-template切换到assign-2**



### 步骤1：

**编写python脚本 run.py实现按照以下要求设置模拟配置参数**：

1）CPU 模型为HW2TimingSimpleCPU 

2）设置workload为HelloWorldWorkload 和DAXPY

```python
from components.processors import HW2MinorCPU
from components.cache_hierarchies import HW2MESITwoLevelCache
from components.memories import HW2DDR3_1600_8x8
from components.boards import HW2RISCVBoard
from components.processors import HW2TimingSimpleCPU
from workloads.roi_manager import exit_event_handler
from workloads.daxpy_workload import DAXPYWorkload
from workloads.hello_world_workload import HelloWorkload
from gem5.simulate.simulator import Simulator


if __name__ == "__m5_main__":
  cpu = HW2TimingSimpleCPU()
  #cpu = HW2MinorCPU(issue_latency=1, int_operation_latency=4, fp_operation_latency=4)
  memory = HW2DDR3_1600_8x8()
  cache = HW2MESITwoLevelCache()
  board = HW2RISCVBoard(clk_freq="4GHz", processor=cpu, cache_hierarchy=cache, memory=memory)
  workload = DAXPYWorkload()
  # workload = HelloWorkload()
  board.set_workload(workload)
  simulator = Simulator(board=board, full_system=False, on_exit_event=exit_event_handler)
  simulator.run()
  print("Finished simulation.")
```

将run.py放到代码区，切换到**gem5**根目录下执行

 `build/RISCV/gem5.opt /home/qzj/code/gem5_assignment/run.py`

### 步骤2：

修改配置脚本更改发射延迟和浮点操作延迟



查看HW2MinorCPU初始参数：

![image-20241105141712486](C:\Users\邱子杰\AppData\Roaming\Typora\typora-user-images\image-20241105141712486.png)

```python
from components.processors import HW2MinorCPU
from components.cache_hierarchies import HW2MESITwoLevelCache
from components.memories import HW2DDR3_1600_8x8
from components.boards import HW2RISCVBoard
from components.processors import HW2TimingSimpleCPU
from workloads.roi_manager import exit_event_handler
from workloads.daxpy_workload import DAXPYWorkload
from workloads.hello_world_workload import HelloWorkload
from gem5.simulate.simulator import Simulator


if __name__ == "__m5_main__":
  cpu = HW2MinorCPU(issue_latency=1, int_operation_latency=4, fp_operation_latency=4)
  memory = HW2DDR3_1600_8x8()
  cache = HW2MESITwoLevelCache()
  board = HW2RISCVBoard(clk_freq="4GHz", processor=cpu, cache_hierarchy=cache, memory=memory)
  workload = DAXPYWorkload()
  board.set_workload(workload)
  simulator = Simulator(board=board, full_system=False, on_exit_event=exit_event_handler)
  simulator.run()
  print("Finished simulation.")
```

执行方式同步骤1



### 步骤3：

修改配置脚本更改整数操作延迟和浮点操作延迟

代码与步骤2相同