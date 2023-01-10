# install efs

## EFS の構成





## EFS のマウント

1. マネジメントコンソールから，ファイルシステムを作成する
   - example_2023_0111
   - VPC: EC2 に合わせる
   - EFS 側の Security Group に注意する
     - ref: [Amazon EFSを使ってみた](https://dev.classmethod.jp/articles/lim-efs-hands-on/)
2. 2049ポートのインバウンドルールを，EFS の Security Group に設定する
   EFS の「ネットワーク」のタブ→「管理」で編集できる
   - name: efs-sg-example_2023_0111
   - vpc: EC2 の VPC に合わせる
   - インバウンドルール
     - port: 2049
     - ソース: 10.0.0.0/16 (VPCのCIDRの範囲に合わせる)
   - アウトバンドルール
     - port: すべて
     - ソース: 0.0.0.0
3. EC2に必要ツールをインストールする
   ```
   $ sudo yum install -y amazon-efs-utils
   ```
   ref: [Amazon EFS クライアントの手動インストール](https://docs.aws.amazon.com/ja_jp/efs/latest/ug/installing-amazon-efs-utils.html)
4. マウントする
   ```
   mkdir /mnt/efs
   sudo mount -t efs -o tls [ID]:/ /mnt/efs <- マネジメントコンソールの EFS のページからの「アタッチ」の項目からコマンドを確認できる
   ```
   
   ref: [EFS マウントヘルパーを使用した Amazon EC2 Linux インスタンスをマウントする](https://docs.aws.amazon.com/ja_jp/efs/latest/ug/mounting-fs-mount-helper-ec2-linux.html)


## アンマウント

```
sudo umount /mnt/efs
```

## マウント状況の確認

```
df -h
```



## ref
- [EFS マウントヘルパーを使用して EFS ファイルシステムをマウントする](https://docs.aws.amazon.com/ja_jp/efs/latest/ug/efs-mount-helper.html)
- [How to attach and mount an EFS to my EC2 instance using AWS CDK](https://stackoverflow.com/questions/71964656/how-to-attach-and-mount-an-efs-to-my-ec2-instance-using-aws-cdk)

