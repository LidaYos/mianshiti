## 服务器启动 ==> npm run dev

## 答题代码
<script>

        const list = new Map()
        for (let i = 0; i < 10000; i++) {
            list.set(i, i)
        }
        console.log('有10000个数据，需要顺序的批量提交到服务器', list.size)

        /**
         * newArr -- 需要将map转为数组，[[0,0],[1,1], ....]; 
         * end -- 数据截取范围（末）;
         * i -- 最终 发送请求需要[0,1,2, ...];
         */
        function setData(arr, end) {
            // 将map转为普通数组
            let newArr = Array.from(arr)
            // 数组长度
            let arrLength = newArr.length
            // 如果数组长度小于等于0，不进行递归
            if (arrLength <= 0) return
            // 截取数据，用于发送到服务器
            let myArr = newArr.splice(0, end)
            arrLength = arrLength - end // 每次进来减掉上次截取的长度
            // 发送请求
            let i = myArr.map(item => item[0])
            send(i)
            // 进行递归
            setData(newArr, end)
        }
        // 一次5000条，发两次
        setData(list, 5000)

        // axios请求
        function send(i) {
            const port = 8081;
            axios.post(`http://localhost:${port}`, {i})
            .then(res => console.log(res.data))
            .catch(error => console.error('Error:', error))
        }
    </script>
