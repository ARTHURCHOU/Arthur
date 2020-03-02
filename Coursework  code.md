    import warnings
    warnings.filterwarnings('ignore')
    import jieba
    import numpy as np
    import pandas as pd
    import re
    import nltk
    import matplotlib.pyplot as plt
    import matplotlib
    from wordcloud import WordCloud

    Metoo = pd.read_csv('MeToo Full data.csv')
    pd.set_option('display.max_columns', None)
    print(Metoo.head())
    print(np.shape(Metoo))

    cleaned = Metoo.dropna()
    print(np.shape(Metoo))

    cleaned = Metoo.dropna(axis=1, how='all')
    print(np.shape(Metoo))

    content=Metoo.tweet.values.tolist()
    '''
    hashtag=[]
    for line in content:
        try:
            segs=re.findall(r'#[a-zA-Z]+',line)
            for seg in segs:
                hashtag.append(seg)
        except:
            print(line)
            continue
    print(hashtag)

    hashtag_df=pd.DataFrame({'hashtag':hashtag})
    #增加计数列
    hashtag_stat=hashtag_df.groupby(by=['hashtag'])['hashtag'].agg({'计数':np.size})
    #将词语数目从大到小排序
    hashtag_stat=hashtag_stat.reset_index().sort_values(by=['计数'],ascending=False)

    print(hashtag_stat)
    '''
    mentions=[]
    for line in content:
        try:
            segs=re.findall(r'@[a-zA-Z0-9]+',line)
            for seg in segs:
                mentions.append(seg)
        except:
            print(line)
            continue
    print(mentions)

    mentions_df=pd.DataFrame({'mentions':mentions})
    #增加计数列
    mentions_stat=mentions_df.groupby(by=['mentions'])['mentions'].agg({'count':np.size})
    #将词语数目从大到小排序
    mentions_stat=mentions_stat.reset_index().sort_values(by=['count'],ascending=False)

    mentions_stat.columns = ['id','weight']
    print(mentions_stat.sum())

    mentions_stat.to_csv('node.csv',index=None)
    mentions_central = mentions_stat[:500]
    mentions_central.to_csv('node_central.csv',index=None)
    mentions_centre = mentions_stat[:50]
    mentions_centre.to_csv('node_centre.csv',index=None)

    mentions_list=[]
    for line in content:
        try:
            segs=re.findall(r'@[a-zA-Z]+',line)
            mentions_list.append(segs)
        except:
            print(line)
            continue
    mentions_list = [i for i in mentions_list if (len(i)!=0)]
    #print(mentions_list)

    import itertools
    import collections

    mentions_cooc = [list(itertools.combinations(mention,2)) for mention in mentions_list]
    mentions_cooc = [i for i in mentions_cooc if (len(i)!=0)]
    print(mentions_cooc[:20])

    cooccurance = list(itertools.chain(*mentions_cooc))
    coo_counts = collections.Counter(cooccurance)
    print(coo_counts.most_common(20))

    coo_list=[]
    list_of_lists=[]
    for item in coo_counts.most_common(52181):
        coo_list.append(item[0][0])
        coo_list.append(item[0][1])
        coo_list.append(item[1])
        list_of_lists.append(coo_list)
        coo_list=[]

    edges_df = pd.DataFrame(list_of_lists,columns=['Source','target','weight'])
    edges_df.to_csv('edges.csv',index=None)
    edges_central = edges_df[:1000]
    edges_central.to_csv('edges_central.csv',index=None)
    edges_centre = edges_df[:50]
    edges_centre.to_csv('edges_centre.csv',index=None)

    import networkx as nx

    G = nx.from_pandas_edgelist(df = edges_df, source = 'Source', target='target', edge_attr=['weight'])
    nx.draw(G, with_labels = False)
    plt.show()
