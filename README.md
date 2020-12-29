# muti-thread
# -*- coding: utf-8 -*-
import time
from muti_thread.muti_thread import thread_create,thread_append,thread_start_and_join
import numpy as np

vec1 = [1,2,3,4,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
vec2 = [5,6,7,8,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]

def my_compute(n=10000):
    for i in range(n):
        dist1 = np.dot(vec1, vec2) / (np.linalg.norm(vec1) * np.linalg.norm(vec2))

class thread_param:
    def __init__(self,index):
        self.counter = 0
        self.index = index

#必须有且只有一个参数,最好是某个类的实例
#注意 , 线程函数里最好不要有import的东西 ，里面最好是执行函数
def thread_func(param):
    param.counter += 1
    #测试1万次
    my_compute()
    # if param.counter> 10000:
    #     print(param.index ,'end')
    #     return -1
    return -1 #// 返回值小于0  退出线程 , 0 继续循环执行线程函数


def main():
    start = float(round(time.time() * 1000))
    resource = thread_create()
    #并发线程
    for i in range(4):
        param = thread_param(i)
        thread_append(resource, thread_func, param)
    #开始并等待线程结束
    thread_start_and_join(resource)
    end = float(round(time.time() * 1000))
    print('run time {}'.format (end-start))

if __name__ == '__main__':
    main()

