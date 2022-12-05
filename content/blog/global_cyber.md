（1）What is your cyber security experience?
I am familiar with cryptography and pwn and used to participate in CTF as traP and thehackerscrew. I studied kernel exploits from kernel modules and eBPF bugs, malware analysis, and Windows forensics at the security camp. I have a weekly reading group of math books on elliptic curve cryptography with professionals in the cryptography field, and I often read papers in the cryptography field. We created a programming language for mini-symbolic execution and IoT security.

（2）What is your English proficiency?
EIKEN Grade 2

（3）セキュリティ・キャンプに参加したことがある人は、参加した大会名と、クラス/トラック/受講講義等を教えてください。 (ない場合は「なし」と記載。)
セキュリティ・キャンプ全国大会2022 オンライン (Cトラック 脅威解析クラス)

（4）公開してる技術系の活動や資料(ブログや、Twitter、GitHub、Slideshare、Speaker Deckなど)がありましたらURL等を記載してください。
https://twitter.com/Anko_9801
https://github.com/anko9801
https://www.slideshare.net/DaikiUsami
https://speakerdeck.com/anko9801
https://qiita.com/Anko_9801

（5）PowerShellマルウェアではよく「IEX」という文字列が利用されています。なぜでしょうか？攻撃者目線で回答してください。
Powershell において IEX は Invoke-Expression のエイリアスとして知られている. Invoke-Expression は文字列を Powershell スクリプトとして評価し実行します. これを用いることでコマンドをそのまま実行するよりも難読化を施すことができたり, ダウンロードしたスクリプトを実行し, ファイル検知を回避することができます. またそれを xor や base64 エンコードすることでセキュリティソフトに検出されにくく, 悪意のあるスクリプトを実行することができます.
https://learn.microsoft.com/ja-jp/powershell/module/microsoft.powershell.utility/invoke-expression?view=powershell-7.3

（6）HP募集要項の申込方法から取得した「FindTheKey.exe」のコードを Python3 に移植してください。（2022年11月21日追加）
```python3
key = [0xCB, 0xF7, 0xFA, 0xBF, 0xE8, 0xF0, 0xF2, 0xFA, 0xF1, 0xBF, 0xF6, 0xF1, 0xBF, 0xD6, 0xDB, 0xDE, 0xBF, 0xF3, 0xF0, 0xF8, 0xF0, 0xBF, 0xF6, 0xEC, 0xBF, 0xD2, 0xFE, 0xFB, 0xFE, 0xF2, 0xFA, 0xBF, 0xFB, 0xFA, 0xBF, 0xD2, 0xFE, 0xF6, 0xF1, 0xEB, 0xFA, 0xF1, 0xF0, 0xF1, 0xB1]

number_for_the_key = int(input("Input a number for the key (0-255):"))

if 0 <= number_for_the_key < 0x100:
    for i in range(3):
        key[i] ^= number_for_the_key

    prefix = b"The"
    flag = True
    for i in range(3):
        if key[i] != prefix[i]:
            flag = False

    if flag:
        for i in range(3,0x2d):
            key[i] ^= number_for_the_key
        print(bytes(key))
    else:
        print("Wrong key!!")
```
