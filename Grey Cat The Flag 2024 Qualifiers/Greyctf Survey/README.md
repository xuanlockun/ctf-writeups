Đây là trang web của challenge này.

![a](/Resource/1.png)


Đây là code của sever:

```js

app.post('/vote', async (req, res) => {
    const {vote} = req.body;
    if(typeof vote != 'number') {
        return res.status(400).json({
            "error": true,
            "msg":"Vote must be a number"
        });
    }
    if(vote < 1 && vote > -1) {
        score += parseInt(vote);
        if(score > 1) {
            score = -0.42069;
            return res.status(200).json({
                "error": false,
                "msg": config.flag,
            });
        }
        return res.status(200).json({
            "error": false,
            "data": score,
            "msg": "Vote submitted successfully"
        });
    } else {
        return res.status(400).json({
            "error": true,
            "msg":"Invalid vote"
        });
    }
})

```

Đại khái là bạn kéo cái thanh ở trên xong nhấn "Submit Vote".
Giả sử bạn kéo vote = 1 thì nó sẽ trả về 
```js
else {
        return res.status(400).json({
            "error": true,
            "msg":"Invalid vote"
        });
}
```
Bạn cần phải kéo trong khoảng -1 đến 1
Nhưng vấn để giả sử bạn kéo vote = 0.5
Thì lúc này score+= parseInt(vote);
Hàm parseInt sẽ lấy phần nguyên nên
```js
    parseInt(0.5) = 0;
```
Mà score = -0.42069; nên sẽ score < 1 
```js
        return res.status(200).json({
            "error": false,
            "data": score,
            "msg": "Vote submitted successfully"
        });
```
Bạn cần phải làm cho score > 1 để in ra flag
Ta thấy 
```js 
score += parseInt(vote); 
```
Lúc này ta sẽ khai thách lỗ hỏng của parseInt
Ta có
```js
parseInt(0.00000001) = 1;
```
## Tại sao lại như vậy ? 
Số 0.00000001 trong JavaScript khi được chuyển đổi thành chuỗi (string) không phải là "0.00000001" mà là "1e-8"
Hàm parseInt() trong JavaScript đọc chuỗi và chuyển đổi thành số nguyên. Điểm đặc biệt ở đây là parseInt() chỉ xử lý cho đến khi gặp một ký tự không phải là số. Trong trường hợp của "1e-7", parseInt() sẽ dừng đọc tại ký tự "e" và trả về số nguyên từ phần đã đọc được, tức là 1.

## Giải
Vậy vô burpe suite và post với vote=0.000000001 thôi
Lần 1 thì Score = -0.42069 + 1 nên chưa lớn hơn 1 được
Nên phải post 2 lần là score > 1 

![2](/Resource/image.png)