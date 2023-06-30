第一题：
package main

import (
 "fmt"
 "io/ioutil"
 "net/http"
 "sync"
 "time"
)

func main() {
 start := time.Now()

 urls := generateURLs(100)
 emails := make([]string, 0)

 var wg sync.WaitGroup
 var mu sync.Mutex

 for _, url := range urls {
  wg.Add(1)
  go func(url string) {
   defer wg.Done()

   resp, err := http.Get(url)
   if err != nil {
    fmt.Println("Error fetching URL:", url)
    return
   }
   defer resp.Body.Close()

   body, err := ioutil.ReadAll(resp.Body)
   if err != nil {
    fmt.Println("Error reading response body:", err)
    return
   }

   // 提取电子邮件地址
   extractedEmails := extractEmails(string(body))

   mu.Lock()
   emails = append(emails, extractedEmails...)
   mu.Unlock()
  }(url)
 }

 wg.Wait()

 // 打印提取到的电子邮件地址
 for _, email := range emails {
  fmt.Println(email)
 }

 elapsed := time.Since(start)
 fmt.Println("完成时间:", elapsed)
}

// 生成URL列表
func generateURLs(count int) []string {
 urls := make([]string, 0)
 baseURL := "https://jsonplaceholder.typicode.com/posts"

 for i := 1; i <= count; i++ {
  urls = append(urls, fmt.Sprintf("%s/%d/comments", baseURL, i))
 }

 return urls
}

// 提取电子邮件地址
func extractEmails(text string) []string {}



第二题：
pip install protobuf
syntax = "proto2";
message Person {
  optional string name = 1;
  optional int32 id = 2;
optional string email = 3;
}
protoc --python_out=. data.proto
import data_pb2

# 创建新的数据对象
data = data_pb2. Person ()
data.name = "John Hu"
data. id = 0630
data. email = 0630@cn.com

# 将数据写入二进制文件
with open("data.bin", "wb") as f:
    f.write(data.SerializeToString())

# 从二进制文件中读取数据
new_data = data_pb2. Person ()
with open("data.bin", "rb") as f:
    new_data.ParseFromString(f.read())

# 打印读取到的数据
print("Name:", new_data.name)
print("Id:", new_data.id)
print("Email:", new_data.email)
