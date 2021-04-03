---
layout: post
title:  "flask"
date:   2021-04-02 09:29
categories: Coding
permalink: /archivers/movie
---


# 플라스크 여러가지 차트
 플라스크에는 여러가지 차트가 있다 word cloud 부터 buble Chart등 여러가지 차트가 있다
 나는 이중 word cloud를 사용하여 내이버 영화 랭킹을 플라스크에 표시해보았다
```python
import requests  
from flask import Flask, json, Response, render_template  
from bs4 import BeautifulSoup  
app = Flask(__name__)  
app.config['JSON_AS_ASCII'] = False  
lc=[]  
@app.route("/")  
def hello():  
    data = dict()  
    response = requests.get('[https://movie.naver.com/movie/sdb/rank/rmovie.nhn](https://movie.naver.com/movie/sdb/rank/rmovie.nhn)')  
    html = response.text  
    soup = BeautifulSoup(html, 'html.parser')  
    ranking = 1  
  for tag in soup.select('div[class=tit3]'):  
        url = tag.get('href')  
        data[(str(51-ranking))] = tag.text.strip()  
  
        ranking = ranking + 1  
  if ranking % 2 == 1:  
            lc.append(tag.text.strip())  
    print(lc)  
    js = json.dumps(data)  
    header = Response(js, status=200, mimetype='application/json')  # 응답코드와 MIME type을 별도로 기술  
  
  return render_template('word_colud1.html', values=data,aa=lc )  
  
  
if __name__ == "__main__":  
    app.run(host="127.0.0.1", port="8080")
    ```
내이버 코드이다 이코드를 word cloud html코드와 연결할것이다
```html 
<!-- [https://falsy.me/d3-js-%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%8B%9C%EA%B0%81%ED%99%94%ED%95%98%EA%B8%B0-3-pie-charts/](https://falsy.me/d3-js-%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%8B%9C%EA%B0%81%ED%99%94%ED%95%98%EA%B8%B0-3-pie-charts/)  -->  
<!doctype html>  
<html lang="ko">  
  
  <head>  
    <title>D3 bar chart example</title>  
    <script src="[https://d3js.org/d3.v3.min.js](https://d3js.org/d3.v3.min.js)"></script>  
<script src="[https://rawgit.com/jasondavies/d3-cloud/master/build/d3.layout.cloud.js](https://rawgit.com/jasondavies/d3-cloud/master/build/d3.layout.cloud.js)" type="text/JavaScript"></script>  
  </head>  
  
  <body>  
   <script>  
    var skillsToDraw = [  
    {% for value in values %}  
    {  
    text: '{{ values[value] }}',  
    size: {{ value }}  
  },  
  {% endfor %}  
];  
  
// Next you need to use the layout script to calculate the placement, rotation and size of each word:  
  
var width = 960;  
var height = 500;  
var fill = d3.scale.category20();  
  
var svg = d3.select("body").append("svg")  
  .attr("width", width)  
  .attr("height", height);  
var svg = d3.select("svg")  
  .append("g")  
  .attr("transform", "translate(" + width / 2 + "," + height / 2 + ")")  
var keywords = {{aa | safe}}  
  
function showCloud(words) {  
  d3.layout.cloud()  
    .size([width, height])  
    .words(words)  
    .rotate(function() {  
      return ~~(Math.random() * 2) * 90;  
    })  
    .font("Impact")  
    .fontSize(function(d) {  
      return d.size;  
    })  
    .on("end", drawSkillCloud)  
    .start();  
}  
showCloud(skillsToDraw);  
  
// Finally implement `drawSkillCloud`, which performs the D3 drawing:  
  
// apply D3.js drawing API  
function drawSkillCloud(words) {  
  var cloud = svg.selectAll("text").data(words)  
  //Entering words  
  cloud.enter()  
    .append("text")  
    .style("font-family", "overwatch")  
    .style("fill", function(d) {  
      return (keywords.indexOf(d.text) > -1 ? "#0700EF" : "#FF002A");  
    })  
    .style("fill-opacity", .5)  
    .attr("text-anchor", "middle")  
    .attr('font-size', 1)  
    .text(function(d) {  
      return d.text;  
    });  
  cloud  
    .transition()  
    .duration(600)  
    .style("font-size", function(d) {  
      return d.size + "px";  
    })  
    .attr("transform", function(d) {  
      return "translate(" + [d.x, d.y] + ")rotate(" + d.rotate + ")";  
    })  
    .style("fill-opacity", 1);  
}  
  
// set the viewbox to content bounding box (zooming in on the content, effectively trimming whitespace)  
setInterval(function() {  
  showCloud(skillsToDraw);  
}, 2000)  
    </script>  
  </body>  
  
</html>```


이것이 word colud html코드이다 연결하면 이런식으로 나오게 된다
![로고](https://gnaud221.github.io/image/word.PNG )
