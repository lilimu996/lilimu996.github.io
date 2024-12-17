# Memcpy踩坑指南


&lt;!--more--&gt;
```c
void readfuc(uint8_t* src,uint16_t size){
    uint8_t rev_data[size&#43;1];
    rev_data[0] = 0xff;
    for(uint8_t i = 0;i &lt; size &#43; 1;i&#43;&#43;){
        rev_data[i] = 0;
    }
    memcpy(src &#43; 1,rev_data &#43; 1,size);
}

void memcpyfuc(){
    uint8_t src[4] = {0xff,0xff,0xff,0xff};
    for(int i = 0;i &lt; 4;i&#43;&#43;){
        printf(&#34;origin data:%X  &#34;,src[i]);
    }
    printf(&#34;\n\r&#34;);
    readfuc(src,3);
    for(int i = 0;i &lt; 4;i&#43;&#43;){
        printf(&#34;transed data:%X  &#34;,src[i]);
    }
}
```
编写代码的时候需要使用到 memcpy ,但是由于理解错误，导致了一个奇怪的 bug ，调试了一个多小时才发现
下面是 memcpy() 的函数声明
```c
void *memcpy(void *str1, const void *str2, size_t n)
```
我想把数组 `int rec_data[4]` 中的后三个数据拷贝到 `int scr[4]` 1 - 3 中
然后我的代码这样写的
```
memcpy(src ,rev_data &#43; 1,3);
```
```scr[4]``` 的初始内容是 ```{0xff,0xff,0xff,0xff}```,```rec_data[4]```的内容是```{0x00,0x00,0x00,0x00}```,我预期拷贝后```scr```中的内容是 ```0Xff 0x00 0x00 0x00```,然而他却是 ```0x00 0x00 0x00 0xff```让我百思不得其解,代码看了一遍又一遍，才发现我这样是把```rev_data```的后三个数据拷贝到```scr```前三个中，真是哭笑不得。

---

> 作者:   
> URL: http://example.org/posts/3470786/  

