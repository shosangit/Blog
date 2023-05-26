# HAL常用函数笔记
## 输出重定向：
+ F1系列：
```c
/* USER CODE BEGIN 1 */
#if 1
#include <stdio.h>

int fputc(int ch, FILE *stream)
{
    /* 堵塞判断串口是否发送完成 */
    while((USART1->SR & 0X40) == 0);

    /* 串口发送完成，将该字符发送 */
    USART1->DR = (uint8_t) ch;

    return ch;
}
#endif
/* USER CODE END 1 */
```
+ 其他系列：

```c
/* USER CODE BEGIN 1 */
#if 1
#include <stdio.h>

int fputc(int ch, FILE *stream)
{
    /* 堵塞判断串口是否发送完成 */
    while((USART1->ISR & 0X40) == 0);

    /* 串口发送完成，将该字符发送 */
    USART1->TDR = (uint8_t) ch;

    return ch;
}
#endif

/* USER CODE END 1 */
```

## GPIO:
+ 初始化
```c
void HAL_GPIO_Init(GPIO_TypeDef *GPIOx, GPIO_InitTypeDef *GPIO_Init);
```
GPIO 引脚功能的初始化，初始化工作模式，包括具体引脚的工作速度、是否复用模式、上下拉等参数。  
+ 恢复初始值
```c
void HAL_GPIO_DeInit(GPIO_TypeDef *GPIOx, uint32_t GPIO_Pin);
```
这个函数的主要功能是将我们在1函数初始化之后的引脚恢复成默认的状态，即各个寄存器复位时的值。这个函数比较容易理解。
+ 读取引脚电平状态
```c
GPIO_PinState HAL_GPIO_ReadPin(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin);
```
这个函数主要功能是读取我们想要知道的引脚的电平状态、函数返回值为0或1。
+ 设置高低电平
```c
void HAL_GPIO_WritePin(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin, GPIO_PinState PinState);
```
这个函数从字面意思来看就是给某个引脚写0或1，但是不要理解成，写1就是使能之类的意思，有些寄存器写1是擦除的意思，这一点要谨记。
+ 反转引脚状态
```c
void HAL_GPIO_TogglePin(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin);
```
这个函数用来翻转某个引脚的电平状态，我用的最多的场合是LED灯的翻转，也就是LED闪烁，哈哈。
+ 锁定管脚值
```c
HAL_StatusTypeDef HAL_GPIO_LockPin(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin);
```
这个函数看函数名称就是锁住的意思，比如说一个管脚的当前状态是1，读管脚值使用锁定，当这个管脚电平变化时保持锁定时的值。
+ 外部中断服务
```c
void HAL_GPIO_EXTI_IRQHandler(uint16_t GPIO_Pin);
```
这个函数是外部中断服务函数，用来响应外部中断的触发，函数实体里面有两个功能，1是清除中断标记位，2是调用下面8要介绍的回调函数。很好理解，我们学习51单片机的时候肯定用过中断服务函数。
+ 中断回调
```c
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin);
```
这个函数是中断回调函数，可以理解为中断函数具体要响应的动作。
## USART(串口):

+ 串口初始化
```c
MX_USART1_UART_Init();
```
功能：在使用hal库串口操作之前先对其进行初始化。

+ 串口发送数据
```c
HAL_UART_Transmit(UART_HandleTypeDef *huart, uint8_t *pData, uint16_t Size, uint32_t Timeout)
```
**功能：**  
串口发送指定长度的数据。如果超时没发送完成，则不再发送，返回超时标志（HAL_TIMEOUT）。

**参数：** 
UART_HandleTypeDef *huart  UATR的别名  
如 :   UART_HandleTypeDef huart1; 别名就是huart1  
*pData      需要发送的数据 
Size    发送的字节数
Timeout   最大发送时间，发送数据超过该时间退出发送

+ 中断接收数据
```c
HAL_UART_Receive_IT(UART_HandleTypeDef *huart, uint8_t *pData, uint16_t Size)
```
**功能：**  
串口中断接收，以中断方式接收指定长度数据。
大致过程是，设置数据存放位置，接收数据长度，然后使能串口接收中断。接收到数据时，会触发串口中断。
再然后，串口中断函数处理，直到接收到指定长度数据，而后关闭中断，进入中断接收回调函数，不再触发接收中断。(只触发一次中断)

**参数：**  
UART_HandleTypeDef *huart      UATR的别名    如 :   UART_HandleTypeDef huart1;   别名就是huart1  
*pData  接收到的数据存放地址  
Size  接收的字节数  

## ADC数模转换:
+ ADC初始化
```c
MX_ADC1_Init();
```
+ ADC转换
```c
HAL_ADC_Start(&hadc1);                    //启动ADC单次转换
HAL_ADC_PollForConversion(&hadc1, 50);    //等待ADC转换完成
smoke_value = HAL_ADC_GetValue(&hadc1);   //读取ADC转换数据
```

## I2C:
+ 阻塞模式向器件发送数据
```c
HAL_I2C_Master_Transmit(I2C_HandleTypeDef *hi2c, uint16_t DevAddress, uint8_t *pData, uint16_t Size, uint32_t Timeout)
```
接口组，设备寄存器地址，向器件发送数据，后面接的是数据大小，时间。函数返回的值为hal状态
+ 阻塞模式从器件读数据
```c
HAL_I2C_Master_Receive(I2C_HandleTypeDef *hi2c, uint16_t DevAddress, uint8_t *pData, uint16_t Size, uint32_t Timeout)
```
从I2c器件读取1字节的数据缓存到data中，接一个timeout时间。函数返回的值为hal状态
+ 阻塞模式读存储器中数据
```c
HAL_I2C_Mem_Read(&hi2c1, AT24C02_ADDR_READ, addr, I2C_MEMADD_SIZE_8BIT, read_buf, 1, 0xFFFF);
```
&hi2c1:指定是哪条线  
AT24C02_ADDR_READ：指的是地址  
addr：指的是从该存储器的地址读出数据  
I2C_MEMADD_SIZE_8BIT：读出数据的长度  
read_buf：将数据读出到内存当中，通常开个数组存起来。  
1： 时间，要读数据占用通道多久。
0xffff：结束标志。  

+ 带中断非阻塞模式发数据

+ 带中断非阻塞模式读数据