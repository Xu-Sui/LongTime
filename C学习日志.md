### 2025.12.08

- 所有 **ctype.h** 中的函数（如 **isdigit**, **isalpha**, **isspace** 等）都必须传入 **unsigned char** 转换后的值或 **EOF**

- 注意**显式加号**对字符串输入的数字的影响

- 溢出可以用更大存储量的变量先行比较，而后存储

- **宏**更适合小工作，不适合做大的功能

- CPU 运算速度极快，故不可再循环内部写入**srand**，这样会导致输出值完全相等

  - 实现更随机的**随机**

    - **C++11**，**<chrono>**，足以获取**纳秒级**时间戳（了解）

      ```c++
      #include <chrono>
      
      unsigned long long nanoseconds() {
      using namespace std::chrono;
      return duration_cast<nanoseconds>(
      	high_resolution_clock::now().time_since_epoch()
      ).count();
      }
      int main() {
      srand(nanoseconds());
      }
      ```

    - **混合种子**，**时间 + 进程ID + 内存地址**（了解）

      ```c
      unsigned int hybrid_seed() {
      	unsigned int seed = 0;
      	seed ^= time(NULL);
      	seed ^= getpid();
      	seed ^= (unsigned int)&seed;
      	return seed;
      }
      int main() {
      	srand(hybrid_seed());
      }
      ```

    - **C++11 <random>**

      ```c++
      #include <random>
      #include <iostream>
      
      int main() {
          // 1. 真随机种子（硬件熵池）
          std::random_device rd;  // 可能调用RDRAND指令
          
      	// 2. 伪随机引擎（Mersenne Twister，周期极长）
      	std::mt19937 gen(rd());  // 用真随机种子初始化
      
      	// 3. 分布器（生成1-100的均匀分布）
      	std::uniform_int_distribution<> dis(1, 100);
      
      	// 4. 使用
      	for (int i = 0; i < 5; i++) {
          	std::cout << dis(gen) << std::endl;  // 高质量随机数
      	}
      }
      ```

### 2025.12.09

- **C语言编译的四个阶段**

  ```bash
  1. 预处理（Preprocessing） ← 宏在这里工作
     - 展开所有 #define 宏
     - 处理 #include 头文件
     - 删除所有注释
  
  2. 编译（Compilation）
     - 把C代码转成汇编
  
  3. 汇编（Assembly）
     - 把汇编转成机器码（.o文件）
  
  4. 链接（Linking）
     - 合并所有.o文件，生成可执行文件
  ```

- **日志书写**

  | 级别      | 含义             | 使用场景                   |
  | --------- | ---------------- | -------------------------- |
  | **DEBUG** | 调试信息         | 开发时打印变量值           |
  | **INFO**  | 正常信息         | 程序启动、用户操作         |
  | **WARN**  | 警告（可恢复）   | 配置缺失、降级处理         |
  | **ERROR** | 错误（不可恢复） | 数据库连接失败、文件不存在 |
  | **FATAL** | 致命错误         | 程序无法继续运行           |
  | **AUDIT** | 审计             | 资金操作、权限变更         |

- 传入的不同类型，通过传入**函数指针**实现！

