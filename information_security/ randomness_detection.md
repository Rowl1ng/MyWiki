## 引言


在许多加密应用中都需要随机和为随机数，比如通用的加密系统都使用随机生成的密钥。许多加密协议也要求在各个点使用伪随机输入，比如生成数字签名的附加部分，或者认证协议生成挑战。
密码算法是构建安全信息系统的核心要素之一，是保障信息与数据机密性、完整性和真实性的重要技术。密码算法检测评估是密码算法研究的重要组成部分，它为密码算法的设计、分析提供客观的量化指标和技术参数，对密码算法的应用具有重要的指导意义．在密码算法的设计和评测过程中，需要从多个方面对其进行检测和分析。“一次一密(One-Time Pad)”是序列密码产生的思想来源，序列密码的核心是通过固定算法，将一串短的密钥序列扩展为长周期的密钥流序列，且密钥流序列在计算能力内应与随机序列不可区分。因此，分析秘钥流序列的随机性是密码算法安全性研究的重要内容，利用随机性检测方法对密码算法进行评测可以为理论分析提供大量参考数据，从而减少理论分析者的工作量，同时可以暴露出用现有的分析方法无法发现的安全漏洞。

%----------------------------------------------------------------------------------------
%	CHAPTER 2
%----------------------------------------------------------------------------------------
\chapterimage{band1.png}

\chapter{检测方法}
对于每一个测试项，先给定一个显著性水平，$[0.001,0.01]$。再给出两个假设：原假设0H(序列是随机的）和备择假设1H(序列是不随机的），然后根据给定统计量的分布函数和统计结果返回一个Pvalue，将它与先前给定的进行比较，从而判断随机性。其中Pvalue是一个概率值，$[0,1]$Pvalue。  

预先定义：
\begin{python}
def sumi(x): return 2 * x - 1
def su(x, y): return x + y
def sus(x): return (x - 0.5) ** 2
def sq(x): return int(x) ** 2
def logo(x): return x * np.log(x)
\end{python}
## 单比特频数检测}\index{First ideas}
该测试关注的是序列中0、1的占比。目的是验证序列是否和真正的随机序列一样，0和1几乎数量相同。
    因此，该测试评估1的占比与$\frac 12$的接近程度，也就是序列中0和1的均衡程度。
\begin{python}
def monobitfrequencytest(binin):
    ss = [int(el) for el in binin]
    sc = map(sumi, ss)
    sn = reduce(su, sc)
    sobs = np.abs(sn) / np.sqrt(len(binin))
    pval = spc.erfc(sobs / np.sqrt(2))
    return pval
\end{python}
## 块内频数检测}\index{Hypothesis}
此检验主要是看M位的子块中“1”码的比例。该检验的目的是判定$M$位的子块内“1”码的频率是否像随机假设下所预期的那样，近似于$M/2$。当$M=1$时，该检测相当于检测1位，即频数（一位）检验。
\begin{python}
def blockfrequencytest(binin, nu=128):
    ss = [int(el) for el in binin]
    tt = [1.0 * sum(ss[xs * nu:nu + xs * nu:]) / nu 
    		for xs in xrange(len(ss) / nu)]
    uu = map(sus, tt)
    chisqr = 4 * nu * reduce(su, uu)
    pval = spc.gammaincc(len(tt) / 2.0, chisqr / 2.0)
    return pval
\end{python}
## 游程检验}\index{Hypothesis}
  此检验主要是看游程的总数，游程指的是一个没有间断的相同数序列，即游程或者是“$1111\ldots$”或者是“$0000\ldots$„”。一个长度为$k$ 的游程包含$k$ 个相同的位。游程检测的目的是判定不同长度的“1”游程的数目以及“0”游程的数目是否跟理想的随机序列的期望值相一致。具体的讲，就是该检验手段判定在这样的“0”“1”子块之间的振荡是否太快或太慢。
\begin{python}
def runstest(binin):
    ss = [int(el) for el in binin]
    n = len(binin)
    pi = 1.0 * reduce(su, ss) / n
    vobs = len(binin.replace('0', ' ').split()) /
    		+ len(binin.replace('1' , ' ').split())
    pval = spc.erfc(abs(vobs-2*n*pi*(1-pi)) /
    		/ (2 * pi * (1 - pi) * np.sqrt(2*n)))
    return pval
\end{python}
## 块内最长游程检验}\index{Hypothesis}
该检验主要是看长度为$M$-bits的子块中的最长“1”游程。这项检验的目的是判定待检验序列的最长“1”游程的长度是否同随机序列的相同。注意：最长“1”游程长度上的一个不规则变化意味着相应的“0”游程长度上也有一个不规则变化，因此，仅仅对“1”游程进行检验室足够的。
\begin{python}
def longestrunones8(binin):
    m = 8
    k = 3
    pik = [0.2148, 0.3672, 0.2305, 0.1875]
    blocks = [binin[xs*m:m+xs*m:] for xs in xrange(len(binin) / m)]
    n = len(blocks)
    counts1 = [xs+'01' for xs in blocks] 
    # append the string 01 to guarantee the length of 1
    counts = [xs.replace('0',' ').split() for xs in counts1] 
    # split into all parts
    counts2 = [map(len, xx) for xx in counts]
    counts4 = [(4 if xx > 4 else xx) /
    			for xx in map(max,counts2)]
    freqs = [counts4.count(spi) for spi in [1, 2, 3, 4]]
    chisqr1 = [(freqs[xx]-n*pik[xx])**2/(n*pik[xx]) /
    			for xx in xrange(4)]
    chisqr = reduce(su, chisqr1)
    pval = spc.gammaincc(k / 2.0, chisqr / 2.0)
    return pval
\end{python}

## 离散傅里叶变换检验}\index{Hypothesis}
本检验主要是看对序列进行分步傅里叶变换后的峰值高度。目的是探测待检验信号的周期性，以此揭示其与相应的随机信号之间的偏差程度。做法是观察超过 95%阈值的峰值数目与低于 5%峰值的数目是否有显著不同。
\begin{python}
def spectraltest(binin):
    n = len(binin)
    ss = [int(el) for el in binin]
    sc = map(sumi, ss)
    ft = sff.fft(sc)
    af = abs(ft)[1:n/2+1:]
    t = np.sqrt(np.log(1/0.05)*n)
    n0 = 0.95*n/2
    n1 = len(np.where(af<t)[0])
    d = (n1 - n0)/np.sqrt(n*0.95*0.05/4)
    pval = spc.erfc(abs(d)/np.sqrt(2))
    return pval
\end{python}
## 非重叠模块匹配检验}\index{Hypothesis}
此检测主要是看提前设置好的目标数据串发生地次数。目的是探测那些产生太多给出的非周期模式的发生器。对于非重叠模块匹配检验以及后面会谈到的重叠模块匹配检验方法，我们都是使用一个 m-bit 的窗口来搜素一个特定的 m-bit 模式。如果这个模式没有被找到，则窗口向后移动一位。如果模式被发现，则窗口移动到一发现的模式的后一位，重复前面的步骤继续搜素下一个模式。
\begin{python}
def nonoverlappingtemplatematchingtest(binin, mat="000000001", num=8):
    n = len(binin)
    m = len(mat)
    M = n/num
    blocks = [binin[xs*M:M+xs*M:] for xs in xrange(n/M)]
    counts = [xx.count(mat) for xx in blocks]
    avg = 1.0 * (M-m+1)/2 ** m
    var = M*(2**-m -(2*m-1)*2**(-2*m))
    chisqr = reduce(su, [(xs - avg) ** 2 for xs in counts]) / var
    pval = spc.gammaincc(1.0 * len(blocks) / 2, chisqr / 2)
    return pval
\end{python}
## 重叠模块匹配检验}\index{Hypothesis}
该检验主要是看提前设定的目标模块发生地数目。检验步骤同非重叠模块匹配检验方法大致一样，不同点在于，发现目标模块后，窗口仅向后移动1位，而后继续搜索。 
\begin{python}
def occurances(string, sub):
    count=start=0
    while True:
        start=string.find(sub,start)+1
        if start>0:
            count+=1
        else:
            return count

def overlappingtemplatematchingtest(binin,mat="111111111",num=1032,numi=5):
    n = len(binin)
    bign = int(n / num)
    m = len(mat)
    lamda = 1.0 * (num - m + 1) / 2 ** m
    eta = 0.5 * lamda
    pi = [pr(i, eta) for i in xrange(numi)]
    pi.append(1 - reduce(su, pi))
    v = [0 for x in xrange(numi + 1)]
    blocks = stringpart(binin, num)
    blocklen = len(blocks[0])
    counts = [occurances(i,mat) for i in blocks]
    counts2 = [(numi if xx > numi else xx) for xx in counts]
    for i in counts2: v[i] = v[i] + 1
    chisqr = reduce(su, [(v[i]-bign*pi[i])** 2 / (bign*pi[i]) for i in xrange(numi + 1)])
    pval = spc.gammaincc(0.5*numi, 0.5*chisqr)
    return pval
\end{python}
## Maurer 的通用统计检验}\index{Hypothesis}
检验主要是看匹配模块间的bit数。目的是检验序列能否在没有信息损耗的条件下被大大的压缩。一个能被大大压缩的序列被认为是一个非随机序列。
\begin{python}
def maurersuniversalstatistictest(binin,l=7,q=1280):
    ru = [
        [0.7326495, 0.690],
        [1.5374383, 1.338],
        [2.4016068, 1.901],
        [3.3112247, 2.358],
        [4.2534266, 2.705],
        [5.2177052, 2.954],
        [6.1962507, 3.125],
        [7.1836656, 3.238],
        [8.1764248, 3.311],
        [9.1723243, 3.356],
        [10.170032, 3.384],
        [11.168765, 3.401],
        [12.168070, 3.410],
        [13.167693, 3.416],
        [14.167488, 3.419],
        [15.167379, 3.421],
        ]
    blocks = [int(li, 2) + 1 for li in stringpart(binin, l)]
    k = len(blocks) - q
    states = [0 for x in xrange(2**l)]
    for x in xrange(q):
        states[blocks[x]-1]=x+1
    sumi=0.0
    for x in xrange(q,len(blocks)):
        sumi+=np.log2((x+1)-states[blocks[x]-1])
        states[blocks[x]-1] = x+1
    fn = sumi / k
    c=0.7-(0.8/l)+(4+(32.0/l))*((k**(-3.0/l))/15)
    sigma=c*np.sqrt((ru[l-1][1])/k)
    pval = spc.erfc(abs(fn-ru[l-1][0]) / (np.sqrt(2)*sigma))
    return pval
\end{python}
## Lempel-Ziv压缩检验}\index{Hypothesis}
\begin{python}
def lempelzivcompressiontest1(binin):
    i = 1
    j = 0
    n = len(binin)
    mu = 69586.25
    sigma = 70.448718
    words = []
    while (i+j)<=n:
       tmp=binin[i:i+j:]
       if words.count(tmp)>0:
           j+=1
       else:
           words.append(tmp)
           i+=j+1
           j=0
    wobs = len(words)
    pval = 0.5*spc.erfc((mu-wobs)/np.sqrt(2.0*sigma))
    return pval
\end{python}
## 线性复杂度检验}\index{Hypothesis}
本检验手段主要是看线性反馈移位寄存器的长度。检验的目的是判定序列的复杂程度是否达到可视为是随 机序列的程度。随机序列的特点是有较长的线性反馈移位寄存器。一个线性反馈移位寄存器太小的话意味着序列非随机。
\begin{python}
def linearcomplexitytest(binin,m=500):
    k = 6
    pi = [0.01047, 0.03125, 0.125, 0.5, 0.25, 0.0625, 0.020833]
    avg = 0.5*m + (1.0/36)*(9 + (-1)**(m + 1)) - (m/3.0 + 2.0/9)/2**m
    blocks = stringpart(binin, m)
    bign = len(blocks)
    lc = ([lincomplex(chunk) for chunk in blocks])
    t = ([-1.0*(((-1)**m)*(chunk-avg)+2.0/9) for chunk in lc])
    vg=np.histogram(t,bins=[-9999999999,-2.5,-1.5,-0.5,0.5,1.5,2.5,9999999999])[0][::-1]
    im=([((vg[ii]-bign*pi[ii])**2)/(bign*pi[ii]) for ii in xrange(7)])
    chisqr=reduce(su,im)
    pval=spc.gammaincc(k/2.0, chisqr/2.0)
    return pval
    
def lincomplex(binin):
    lenn=len(binin)
    c=b=np.zeros(lenn)
    c[0]=b[0]=1
    l=0
    m=-1
    n=0
    u=[int(el) for el in binin] # the input string as numbers, to generate the dot product
    p=99
    while n<lenn:
        v=u[(n-l):n] # was n-l..n-1
        v.reverse()
        cc=c[1:l+1] # was 2..l+1
        d=(u[n]+np.dot(v,cc))%2
        if d==1:
            tmp=c
            p=np.zeros(lenn)
            for i in xrange(0,l): # was 1..l+1
                 if b[i]==1:
                     p[i+n-m]=1
            c=(c+p)%2;
            if l<=0.5*n: # was if 2l <= n
                 l=n+1-l
                 m=n
                 b=tmp
        n+=1
    return l
\end{python}
## 二元矩阵秩检验}\index{Hypothesis}
\begin{python}
def binarymatrixranktest(binin,m=32,q=32):
    p1 = 1.0
    for x in xrange(1,50): p1*=1-(1.0/(2**x))
    p2 = 2*p1
    p3 = 1-p1-p2;
    n=len(binin)
    u=[int(el) for el in binin] # the input string as numbers, to generate the dot product
    f1a = [u[xs*m:xs*m+m:] for xs in xrange(n/m)]
    n=len(f1a)
    f2a = [f1a[xs*q:xs*q+q:] for xs in xrange(n/q)]
    # r=map(matrank,f2a)
    r=map(mrank,f2a)
    n=len(r)
    fm=r.count(m);
    fm1=r.count(m-1);
    chisqr=((fm-p1*n)**2)/(p1*n)+((fm1-p2*n)**2)/(p2*n)+((n-fm-fm1-p3*n)**2)/(p3*n);
    pval=np.exp(-0.5*chisqr)
    return pval
    
def mrank(matrix): # matrix rank as defined in the NIST specification
    m=len(matrix)
    leni=len(matrix[0])
    def proc(mat):
        for i in xrange(m):
            if mat[i][i]==0:
                for j in xrange(i+1,m):
                    if mat[j][i]==1:
                        mat[j],mat[i]=mat[i],mat[j]
                        break
            if mat[i][i]==1:
                for j in xrange(i+1,m):
                    if mat[j][i]==1: mat[j]=[mat[i][x]^mat[j][x] for x in xrange(leni)]
        return mat
    maa=proc(matrix)
    maa.reverse()
    mu=[i[::-1] for i in maa]
    muu=proc(mu)
    ra=np.sum(np.sign([xx.sum() for xx in np.array(mu)]))
    return ra
\end{python}

## 近似熵检验}\index{Hypothesis}
同序列检验一样，近似熵检验主要看的也是整个序列中所有可能的重叠 m-bit 模式的频率。目的是将两相邻长度(m和m+1)的重叠子块的频数与随机情况下预期的频数相比较。
\begin{python}
def aproximateentropytest(binin, m=10):
    n = len(binin)
    f1a = [(binin + binin[0:m - 1:])[xs:m + xs:] for xs in xrange(n)]
    f1 = [[xs, f1a.count(xs)] for xs in sorted(set(f1a))]
    f2a = [(binin + binin[0:m:])[xs:m + 1 + xs:] for xs in xrange(n)]
    f2 = [[xs, f2a.count(xs)] for xs in sorted(set(f2a))]
    c1 = [1.0 * f1[xs][1] / n for xs in xrange(len(f1))]
    c2 = [1.0 * f2[xs][1] / n for xs in xrange(len(f2))]
    phi1 = reduce(su, map(logo, c1))
    phi2 = reduce(su, map(logo, c2))
    apen = phi1 - phi2
    chisqr = 2.0 * n * (np.log(2) - apen)
    pval = spc.gammaincc(2 ** (m - 1), chisqr / 2.0)
    return pval
\end{python}
## 累加和检验}\index{Hypothesis}
该检验主要是看随机游动的最大偏移。随机游动被定义为序列中调整后的-1，+1的累加和。检验的目的是判定序列的累加和相对于预期的累加和过大还是过小。这个累加和可被看做随机游动。对于随机序列，随机游动的偏离应该在0附近。而对于非随机序列，这个随机游动偏离将会比0大很多。
\begin{python}
def cumultativesumstest(binin):
    n = len(binin)
    ss = [int(el) for el in binin]
    sc = map(sumi, ss)
    cs = np.cumsum(sc)
    z = max(abs(cs))
    ra = 0
    start = int(np.floor(0.25 * np.floor(-n / z) + 1))
    stop = int(np.floor(0.25 * np.floor(n / z) - 1))
    pv1 = []
    for k in xrange(start, stop + 1):
        pv1.append(sst.norm.cdf((4 * k + 1) * z / np.sqrt(n)) - sst.norm.cdf((4 * k - 1) * z / np.sqrt(n)))
    start = int(np.floor(0.25 * np.floor(-n / z - 3)))
    stop = int(np.floor(0.25 * np.floor(n / z) - 1))
    pv2 = []
    for k in xrange(start, stop + 1):
        pv2.append(sst.norm.cdf((4 * k + 3) * z / np.sqrt(n)) - sst.norm.cdf((4 * k + 1) * z / np.sqrt(n)))
    pval = 1
    pval -= reduce(su, pv1)
    pval += reduce(su, pv2)

    return pval
\end{python}
## 序列检验}\index{Hypothesis}
本检验主要是看整个序列中所有可能的重叠m-bit模式的频率，目的是判定$2^m$个m-bit重叠模式的数目是否跟随机情况下预期的值相近似。随机序列具有均匀性也就是说对于每个m-bit模式其出现的概率应该是一样的。当$m=1$时等价于频数检验。
\begin{python}
def serialtest(binin, m=16):
    n = len(binin)
    hbin=binin+binin[0:m-1:]
    f1a = [hbin[xs:m+xs:] for xs in xrange(n)]
    oo=set(f1a)
    f1 = [f1a.count(xs)**2 for xs in oo]
    f1 = map(f1a.count,oo)
    cou=f1a.count
    f2a = [hbin[xs:m-1+xs:] for xs in xrange(n)]
    f2 = [f2a.count(xs)**2 for xs in set(f2a)]
    f3a = [hbin[xs:m-2+xs:] for xs in xrange(n)]
    f3 = [f3a.count(xs)**2 for xs in set(f3a)]
    psim1 = 0
    psim2 = 0
    psim3 = 0
    if m >= 0:
        suss = reduce(su,f1)
        psim1 = 1.0 * 2 ** m * suss / n - n
    if m >= 1:
        suss = reduce(su,f2)
        psim2 = 1.0 * 2 ** (m - 1) * suss / n - n
    if m >= 2:
        suss = reduce(su,f3)
        psim3 = 1.0 * 2 ** (m - 2) * suss / n - n
    d1 = psim1-psim2
    d2 = psim1-2 * psim2 + psim3
    pval1 = spc.gammaincc(2 ** (m - 2), d1 / 2.0)
    pval2 = spc.gammaincc(2 ** (m - 3), d2 / 2.0)
    return [pval1, pval2]
\end{python}
## 随机游动检验}\index{Hypothesis}
本检验主要是看一个累加和随机游动中具有 K 个节点的循环的个数。累加和随机游动由于将关于“0”，“1”的部分和序列转化成适当的“-1”、“+1”序列产生的。一个随机游动循环由单位步长的一个序列组成，这个序列的起点和终点均是 0。该检验的目的是确定在一个循环内的特殊状态对应的节点数是否与在随机序列中预计达到的节点数相背离。实际上，这个检验由八个检验（和结论）组成，一个检验和结论对应着一个特定的状态：-4，-3，-2，-1和+1，+2，+3，+4 。
\begin{python}
def randomexcursionstest(binin):
    xvals=[-4, -3, -2, -1, 1, 2, 3, 4]
    ss = [int(el) for el in binin]
    sc = map(sumi,ss)
    cumsum = np.cumsum(sc)
    cumsum = np.append(cumsum,0)
    cumsum = np.append(0,cumsum)
    posi=np.where(cumsum==0)[0]
    cycles=([cumsum[posi[x]:posi[x+1]+1] for x in xrange(len(posi)-1)])
    j=len(cycles)
    sct=[]
    for ii in cycles:
        sct.append(([len(np.where(ii==xx)[0]) for xx in xvals]))
    sct=np.transpose(np.clip(sct,0,5))
    su=[]
    for ii in xrange(6):
        su.append([(xx==ii).sum() for xx in sct])
    su=np.transpose(su)
    pikt=([([pik(uu,xx) for uu in xrange(6)]) for xx in xvals])
    # chitab=1.0*((su-j*pikt)**2)/(j*pikt)
    chitab=np.sum(1.0*(np.array(su)-j*np.array(pikt))**2/(j*np.array(pikt)),axis=1)
    pval=([spc.gammaincc(2.5,cs/2.0) for cs in chitab])
    return pval
\end{python}
## 随机游动状态频数检验}\index{Hypothesis}
该检验主要是看累计和随机游动中经历的特殊状态的总数。检验目的是判定随机游动中实际经历多个状态的值与预期值之间的偏离程度。该检验实际上是十八个检验（和结论），每个状态对应着一个检验和一个结论。这些状态分别是：-9，-8，-7，-6，-5，-4，-3，-2，-1和+1，+2，+3，+4，+5，+6，+7，+8。
\begin{python}
def randomexcursionsvarianttest(binin):
    ''' 
    The focus of this test is the number of times that a particular state occurs 
    in a cumulative sum random walk. The purpose of this test is to detect deviations 
    from the expected number of occurrences of various states in the random walk.
    '''
    ss = [int(el) for el in binin]
    sc = map(sumi, ss)
    cs = np.cumsum(sc)
    li = []
    for xs in sorted(set(cs)):
        if np.abs(xs) <= 9:
            li.append([xs, len(np.where(cs == xs)[0])])
    j = getfreq(li, 0) + 1
    pval = []
    for xs in xrange(-9, 9 + 1):
        if not xs == 0:
            # pval.append([xs, spc.erfc(np.abs(getfreq(li, xs) - j) / np.sqrt(2 * j * (4 * np.abs(xs) - 2)))])
            pval.append(spc.erfc(np.abs(getfreq(li, xs) - j) / np.sqrt(2 * j * (4 * np.abs(xs) - 2))))
    return pval
\end{python}
%----------------------------------------------------------------------------------------
%	CHAPTER 3
%----------------------------------------------------------------------------------------

\chapterimage{boat.png}

\chapter{结果分析}
随机数生成方法：
\begin{python}
def randgen(num):
    rn = open('/dev/urandom', 'r')
    random_chars = rn.read(num / 2)
    stream = ''
    for char in random_chars:
        c = ord(char)
        for i in range(0, 2):
            stream += str(c >> i & 1)
    return stream
\end{python}
利用上述生成方法生成了1000个长度为1000000的样本。测试结果如下：
\begin{enumerate}
\item 通过单比特频数检测的样本数：994。通过测试！
\item 通过块内频数检测的样本数：988。通过测试！
\item 通过游程检测的样本数：989。通过测试！
\item 通过块内最长游程检测的样本数：994。通过测试！
\item 通过累加和检测的样本数：992。通过测试！
\item 通过线性复杂度检测的样本数：994。通过测试！
\item 通过Maurer通用统计检测的样本数：987。通过测试！
\item 通过离散傅立叶检测的样本数：986。通过测试！
\end{enumerate}