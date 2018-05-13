# 20180514_python_Practice
# Python3勉強会
## 環境づくり
OS環境が異なることもあり、前回好評だったCloud9で環境と立ち上げて下さい。手順は割愛します。

以下のコマンドを入力してみると`Python 2.7.13`と表示されます。これはデフォルトで入っているpythonのバージョンです。今回はpython3を対象としているのでpython3をインストールしていきます。python2とpython3は記法も異なるので別のものと考えておいて下さい。

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

```
pyenv versions
```

`which python`を入力すると`python='python27'`とAliasがはいっています。これはCloud9の設定のため、'.bashrc'で定義されているAlias設定を消します。

```
cd /home/ec2-user
vi .bashrc
```

「AWS Cloud9」の「Preference」を開き「Python Support」のPython Versionを'Python3'に変更します。
