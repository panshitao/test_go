package main

import (
	"fmt"
)

func test(mid_list []string, ch chan int, start int, size int, mapMidNotHcRedis *map[string]string){
	
	for i:=start;i<len(mid_list) && i<start+size;i++{
		(*mapMidNotHcRedis)[mid_list[i]] = "1"
	}
	ch <- 1
}

func test2(mid_list []string, ch chan int, start int, size int){
	
	for i:=start;i<len(mid_list) && i<start+size;i++{
		
	}
	ch <- 1
}

func main(){

	mid_list:=[]string{"123456", "234345","33455677","11345","456788","334566868","3454657","3435","4t5","4531","45"}
	test_map := make(map[string]string)
	ch_size := 3
    chs := make([] chan int, ch_size)
	batchsize := len(mid_list)/ch_size + 1
	for i :=0;i<len(mid_list);i+=batchsize{
		ch_id := i / batchsize
        chs[ch_id] = make(chan int)
		go  test(mid_list, chs[ch_id], ch_id, batchsize, &test_map)
		// go  test2(mid_list, chs[ch_id], ch_id, batchsize)
	}
	for _, ch := range(chs) {
		<-ch
	}
	// for i:=0;i<ch_size;i++{
	// 	fmt.Println(chs[i])
	// }
	for key:=range test_map{
		fmt.Println(key, test_map[key])
	}

}
