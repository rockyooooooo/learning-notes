# shell script progress bar

1. 很多鬼神方式
    
    [How to add a progress bar to a shell script?](https://stackoverflow.com/questions/238073/how-to-add-a-progress-bar-to-a-shell-script)
    
2. 簡單版
    
    [How to add a progress bar to a shell script?](https://stackoverflow.com/a/28044986)
    
    ```bash
    #!/bin/bash
    # 1. Create ProgressBar function
    # 1.1 Input is currentState($1) and totalState($2)
    function ProgressBar {
    # Process data
        let _progress=(${1}*100/${2}*100)/100
        let _done=(${_progress}*4)/10
        let _left=40-$_done
    # Build progressbar string lengths
        _fill=$(printf "%${_done}s")
        _empty=$(printf "%${_left}s")
    
    # 1.2 Build progressbar strings and print the ProgressBar line
    # 1.2.1 Output example:
    # 1.2.1.1 Progress : [########################################] 100%
    printf "\rProgress : [${_fill// /\#}${_empty// /-}] ${_progress}%%"
    
    }
    
    # Variables
    _start=1
    
    # This accounts as the "totalState" variable for the ProgressBar function
    _end=100
    
    # Proof of concept
    for number in $(seq ${_start} ${_end})
    do
        sleep 0.1
        ProgressBar ${number} ${_end}
    done
    printf '\nFinished!\n'
    ```
    
3. 有 spinner 版
    
    [基于shell实现linux下命令行进度条_ronnie88597的博客-CSDN博客_shell进度条直到命令结束](https://blog.csdn.net/weixin_46222091/article/details/104855431)
    
    ```bash
    #!/bin/bash
    function ProgressBar {
        if [ -z $1 ] || [ -z $2 ]; then
            echo "[E]>>>Missing parameter. \$1=$1, \$2=$2"
            return
        fi
    
        pro=$1 # 进度的整型数值
        total=$2 # 总数的整型数值
        if [ $pro -gt $total ];then
            echo "[E]>>>It's impossible that 'pro($pro) > total($total)'."
            return
        fi
        arr=('|' '/' '-' '\\')
        local percent=$(($pro * 100 / $total))
        #echo "pro=$pro, percent=$percent"
        local str=$(for i in `seq 1 $percent`;do printf '=';done)
        let index=pro%4
        printf "[%-100s] [%d%%] [%c]\r" "$str" "$percent" "${arr[$index]}"
        if [ $percent -eq 100 ];then
            echo
        fi
    }
    
    # ---下面是测试代码---
    # 总数为200
    # 进度从1增加到200
    # 循环调用ProgressBar函数，直到进度增加到200结束
    Tot=200
    for i in `seq 1 $Tot`;do
        ProgressBar $i $Tot
        sleep 0.1
    done
    # ————————————————
    # 版权声明：本文为CSDN博主「ronnie88597」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
    # 原文链接：https://blog.csdn.net/weixin_46222091/article/details/104855431
    ```