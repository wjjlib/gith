# gith


## nodejs实现的简单接口

<pre>
var http = require('http');
var mysql      = require('mysql');
var connection = mysql.createConnection({
    host     : 'rm-bp1w72suk8pc5h6mk.mysql.rds.aliyuncs.com',
    user     : 'r85u40z94p',
    password : 'Mafeng11',
    database: 'r85u40z94p'
});
//开始你的mysql连接
connection.connect();

var server = http.createServer(function (req, res) {
    //如果你发一个GET到http://127.0.0.1:9000/test
    var url_info = require('url').parse(req.url, true);
    //检查是不是给/test的request
    if(url_info.pathname === '/test'){
        res.writeHead(200, {'Content-Type': 'text/plain'});
        

                connection.query('SELECT * FROM `r85u40z94p`.`user`  order by rand()  LIMIT 5',function(err,rows,fields){
                    //处理你的结果
                   // res.end(rows.constructor);
                    // 输出结果
                    res.end(JSON.stringify(rows));

                    console.log(rows.constructor);
                    console.log(typeof(rows));
                    res.end(rows.join);
                    console.log(err);
                    console.log(fields);
                });
    }
    //这个是用来回复上面那个post的，显示post的数据以表示成功了。你要是有别的目标，自然不需要这一段。
    else{
        req.pipe(res);
    }
});
server.listen(9000, '127.0.0.1');
//在server关闭的时候也关闭mysql连接
server.on('close',function(){
    connection.end();
});
console.log('listening on port  9000');
</pre>