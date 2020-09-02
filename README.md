# word2vec_wiki
word2vec_wiki
https://dumps.wikimedia.org/zhwiki/latest/   可下载最新语料
http://www.mamicode.com/info-detail-1699780.html

1、先将wiki的资料转为文本txt
    python process_wiki.py zhwiki-latest-pages-articles.xml.bz2 wiki.zh.text


2、进入解压后的opencc的目录，打开dos窗口，输入
opencc -i wiki.zh.text -o wiki.zh.jian.text -c t2s.json

opencc -i F:\pywork\work\pywork\wiki\wiki.zh.text -o wiki.zh.jian.text -c t2s.json

3、分词
fenci.py

4、训练word2vec模型
        python train_word2vec_model.py wiki.zh.jian.seg.txt wiki.zh.text.model wiki.zh.text.vector

得到 4个文件  wiki.zh.text.model    wiki.zh.text.model.syn1neg.npy   
wiki.zh.text.model.wv.syn0.npy    wiki.zh.text.vector


5、测试训练好的模型
注：因为gensim版本更新的问题，如果下面这个load有问题，可以使用新的接口：model = gensim.models.word2vec.Word2Vec.load(MODEL_PATH)
In [3]: model = gensim.models.Word2Vec.load_word2vec_format("wiki.zh.text.vector", binary=False)
 
model.most_similar("queen")


import gensim 
model = gensim.models.Word2Vec.load("wiki.en.text.model")
model.most_similar("man")

model = gensim.models.Word2Vec.load('wiki.zh.text.model')
words = model.most_similar('纺织')
print(words)
print(model.most_similar(positive=['皇上','国王'],negative=['皇后']))
print(model.doesnt_match('太后 妃子 贵人 人才'.split()))
print(model.similarity('书籍','书本'))

在代码前面加：
reload(sys)
sys.setdefaultencoding('utf8')

可在这里下载最新语料
https://dumps.wikimedia.org/zhwiki/latest/
zhwiki-latest-pages-articles.xml.bz2

中间可能回到这样的问题
UnicodeDecodeError: 'utf8' codec can't decode bytes in position 5394-5395: invalid continuation byte

google了一下，大致是文件中包含非utf-8字符，又用iconv处理了一下这个问题：

iconv -c -t UTF-8 < wiki.zh.text.jian.seg > wiki.zh.text.jian.seg.utf-8

这样基本上就没问题了，执行：
python train_word2vec_model.py wiki.zh.text.jian.seg.utf-8 wiki.zh.text.model wiki.zh.text.vector
import gensim
