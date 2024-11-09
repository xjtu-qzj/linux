## 实验三：矩阵乘法分析模拟

### 步骤1：

1. 先将 gem assignment template 切换到 assign 1

2. 在实验二run .py 的基础上修改模型配置

   ```python
   from components.boards import HW1RISCVBoard
   from components.cache_hierarchies import HW1MESITwoLevelCache
   from components.processors import HW1TimingSimpleCPU
   from components.processors import HW1MinorCPU
   from components.memories import HW1DDR3_1600_8x8
   from components.memories import HW1DDR3_2133_8x8
   from components.memories import HW1LPDDR3_1600_1x32
   from workloads.mat_mul_workload import MatMulWorkload
   from gem5.simulate.simulator import Simulator
   
   
   if __name__ == "__m5_main__":
     clk = "1GHz"     ## "1GHz"   "2GHz"   "4GHz"
     cpu = HW1TimingSimpleCPU()
     #cpu = HW1MinorCPU()
     memory = HW1DDR3_1600_8x8()
     # memory = HW1DDR3_2133_8x8()
     # memory = HW1LPDDR3_1600_1x32()
     cache = HW1MESITwoLevelCache()
     board = HW1RISCVBoard(clk_freq=clk, processor=cpu, cache_hierarchy=cache, memory=memory)
     workload = MatMulWorkload(224)
     board.set_workload(workload)
     simulator = Simulator(board=board, full_system=False)
     simulator.run()
     print("Finished simulation.")
   
   ```

   

3. 完成6个配置的仿真实验

   在 gem5 根目录运行以下命令：

   build/RISCV/gem5.opt /home/qzj/code/gem5_assignment/run.py

   执行结果如下图所示：

   ![image-20241028172247823](C:\Users\邱子杰\AppData\Roaming\Typora\typora-user-images\image-20241028172247823.png)

   记录各次仿真实验结果（主要为运行时间）。
   
   编写数据可视化python脚本
   
   ```python
   import matplotlib.pyplot as plt
   
   run_times = {
       "HW1TimingSimpleCPU@1GHz": 287.841,
       "HW1TimingSimpleCPU@2GHz": 144.143,
       "HW1TimingSimpleCPU@4GHz": 72.2922,
       "HW1MinorCPU@1GHz": 133.542,
       "HW1MinorCPU@2GHz": 66.9952,
       "HW1MinorCPU@4GHz": 33.7088
   }
   
   cpu_models = ["HW1TimingSimpleCPU", "HW1MinorCPU"]
   clock_frequencies = ["1GHz", "2GHz", "4GHz"]
   
   plt.figure(figsize=(10, 6))
   
   bar_width = 0.35  
   index = range(len(clock_frequencies))  
   
   for i, model in enumerate(cpu_models):
       times = [run_times[f"{model}@{freq}"] for freq in clock_frequencies]
       plt.bar([x + bar_width * i for x in index], times, bar_width, label=model)
       for j, time in enumerate(times):
           plt.text(j + bar_width * i, time + 0.01, f'{time:.2f}', ha='center')
   
   plt.title('Simulation Run Times for Different CPU Models and Clock Frequencies')
   plt.xlabel('Clock Frequency')
   plt.ylabel('Run Time (ms)')
   plt.xticks([x + bar_width for x in index], clock_frequencies)
   
   plt.legend()
   
   plt.tight_layout()
   plt.show()
   ```
   
   

### 步骤2：

操作同步骤1