# Python3勉強会

## 環境づくり
OS環境が異なることもあり、前回好評だったCloud9で環境と立ち上げて下さい。手順は割愛します。

以下のコマンドを入力してみると`Python 2.7.13`と表示されます。これはデフォルトで入っているpythonのバージョンです。今回はpython3を対象としているのでpython3をインストールしていきます。ちまたでpythonとよく聞きますがpython2とpython3の二種類があり記法も異なるので注意して下さい。

```
python --version
```

まず、pyenvをインストールします。これはpyenvはpythonのバージョンなど環境をを切り替えることができるようになります。

```
git clone git://github.com/yyuu/pyenv.git ~/.pyenv
```

以下のコマンドでパスを通します。
```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc
$ source ~/.bashrc
```

以下のコマンドでインストールできるpythonのバージョンを確認します。

```
pyenv install -l
```

以下のコマンドで`3.6.5`のバージョンをインストールします。（１~２分かかります）

```
pyenv install 3.6.5
```

pyenvで設定されているpythonのバージョンを調べます。

```
pyenv versions
```

`system`環境に設定されていると思うので、以下のコマンドで環境設定を`3.6.5`に変更します。

```
pyenv global 3.6.5
``` 

`３．６．５`に変更されていることを確認して、これで準備OK！！！
と言いたいところですが以下のコマンドを実行すると`Python 2.7.13`のままになっています。
これはcloud9の設定のようです。
http://servernameis.xsrv.jp/CryptoCurrencyAutoTrader/2018/03/24/cloud9-changepython3/

```
python --version
```

`which python`を入力すると`python='python27'`とAliasがはいっています。これはCloud9の設定のため、'.bashrc'で定義されているAlias設定を消します。

```
cd /home/ec2-user
vi .bashrc
```

「AWS Cloud9」の「Preference」を開き「Python Support」のPython Versionを'Python3'に変更します。

## Hello Worldの出力
以下のコマンドを実行し'helloWorld.py'ファイルを作成します。

```
cd /home/ec2-user/environment/
```

```
mkdir myapp
```

```
cd myapp
```

```
touch hello_world.py
```

viコマンドで以下のように編集します。

```python:hello_world.py

#! python
print('hello world')
```

編集が終わったら以下のコマンドで実行します。
python hello_world.py

## おまけ


以下を入力してREPLを起動します。
```
python
```

以下を入力します。pythonの心得が表示されます。
```
import this
```

## WEBスクレイピング

pipコマンドでbeautifulsoup4,requestsをインストールします。
```
pip install beautifulsoup4
pip install requests
```

インストールが終わったら以下のquestion1ファイルを作成します。

```python:question1.py
#! python3
# google検索で上位の結果のURLを取得する
import requests, sys, bs4

print('GooGling...')
res = requests.get('http://google.com/search?q=' + ' '.join(sys.argv[1:]))
res.raise_for_status()

soup = bs4.BeautifulSoup(res.text)
link_elems = soup.select('.r a')

num_open = min(5, len(link_elems))
for i in range(num_open):
    print('http://google.com' + link_elems[i].get('href'))
```
以下のコマンドで検索結果を取得することができます。※seleniumが使えればprint部分をリンクで開く設定にすれば、複数ページを開くことができる。

```
python question1.py <検索したい言葉>
```

# 天気予報取得
以下のサイトより天気予報を取得して表示します。
<http://weather.livedoor.com/weather_hacks/webservice>

```python:weather.py
#!python3
# coding: utf-8

import requests
import json

url = 'http://weather.livedoor.com/forecast/webservice/json/v1'
city = {'city': '130010'} # Tokyo
data = requests.get(url, params = city).json()
print(data['title'])
for weather in data['forecasts']:
    print(weather['dateLabel'] + ':' + weather['telop'])
```

## カウントダウンで処理実行
3秒毎に経過時間を出力するスクリプトです。

```python:countdown.py
import time,subprocess,sys

time_left = 3
while time_left > 0:
    print(time_left, end='')
    time.sleep(1)
    time_left = time_left - 1
    print('秒経ちました')
```
実行できたなら、3秒後にweather.pyを実行するように改良します。

```python:countdown.py
#! python3
#　3秒後にquestion2.pyを実行するスクリプト

import time,subprocess,sys

time_left = 3
while time_left > 0:
    time.sleep(1)
    time_left = time_left - 1

subprocess.Popen([sys.executable, 'weather.py'])
```

一応実行できるのですが、処理結果がイマイチに見えるかもです。
スレッド絡みは勉強不足なので分かる人おしえて下さい。
