
#改写R 包 copynumber 里面的plotHeatmap函数，实现以下效果
![CNV热图](https://github.com/wangweifeng2018/plotCNVHeatmap_genome/blob/master/CNV_heatmap.png)

#Step1. 下载 Bioconductor-copynumber 源码文件，
-----------------
```
$wget http://www.bioconductor.org/packages/release/bioc/src/contrib/copynumber_1.26.0.tar.gz
$tar -zxvf copynumber_1.26.0.tar.gz
$cd copynumber/R/
```
#Step2.修改源码文件
-----------------
#修改addChromlines.r文件在底部添加染色体信息
-----------------
(1)注释掉 row 67-69，将所有染色体标签移到热图下方。
```
        #Plot half at bottom, half at top:
        #bot <- seq(1,length(chrom.mark),2)
        #top <- seq(2,length(chrom.mark),2)
        #mtext(chrom.names[bot],side=1,line=arg$chrom.line[1],at=at[bot],cex=arg$chrom.cex)
        text(x = at[seq(length(chrom.mark))-0.5],y=rep(-3,length(chrom.mark)),labels=chrom.names,pos=1,cex=arg$chrom.cex)
 ```
 (2)row 70 后面紧接着添加如下代码，在热图绘制染色体条带。
 ```
        # plot chromosome strip at the bottom of the figure
        xleft <- chrom.mark[1:(length(chrom.mark)-1)]
        xright <- chrom.mark[2:(length(chrom.mark))]
        par(col = "black",bty="n")
        rect(xleft,c(rep(-3,length(chrom.mark)-1)),xright,c(rep(0,length(chrom.mark)-1)),col = rep(c("black","white"),length(chrom.mark)-1))
 ```
#修改 plotHeatmap.r 文件添加Legend
------------
在Row 181 后面添加如下代码
```
    # Legend
    colfunc <- colorRampPalette(c("dodgerblue", "white","red"))
    rect(c(300:800),rep(-13,500),c(301:801),rep(-10,500),col=colfunc(500),cex=1,border=NA)
    text(x=c(300,550,800),y=rep(-14,3),pos=1,labels=c(lower.lim,0,upper.lim),cex=1)
```
