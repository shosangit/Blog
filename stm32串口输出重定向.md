重定向函数如下，将函数放到任意不会被cube mx初始化覆盖的区块下就好，要记得

先选上micro lib库

```c
int fputc(int ch,FILE *f)
{
    HAL_UART_Transmit(&hlpuart1,(uint8_t*)&ch,1,0xFFFF);
	return ch;
}
```
