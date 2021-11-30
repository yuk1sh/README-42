# TEST
- [x] lsblk

# OSの導入／設定
- [x] UTMを使用
	- M1チップを用いたMacではVirtualBoxが使えないため
- [x] 最新の安定(stable)版のDebianを導入（unstable版、testing版は不可）
	- 予期せぬエラーや特殊な仕様がありうるため安定板を選択
	- Debianは初心者も導入しやすいのでこちらを選択
- [x] AppArmorを導入

# UTMでDebianを動かすために仮想マシンをつくる（初期設定込み）
- [x] メモリは1024MB
	- ［理由］Desktop環境を作らないため、軽くて済む
- [x] Drivesそれぞれ10GB（デフォルト）
- [x] 仮想マシンリストでの表示を Generic から Operationg System へ変更
- [x] SystemのArchitectureはデフォルト
- [x] 言語は`English`に設定する
	- ［理由］文字化け防止策
- [x] Location → other → Asia → Japan
- [x] keymapは`Japanese`に設定する
- [x] Hostnameは`42でのユーザ名 + 42`で設定する。以降`user42`と記す。
- [x] Domainは設定しない。
- [x] ログイン名もHostnameと同様のルールで設定する。
- [x] 一時パスワードは`pass`
- [x] Partitionを設定
	0. Guided - use entire disk and set up LVM → そのままEnter
	1. Separate /home partition → <Yes>
	2. sdaに割り当てるのは例を参考に8GB → <Continue> → <No>
	3. swap_1 の #1 を選択 → Use to: do not use
	4. root の #1 を選択 → Use to: do not use
	5. home の #1 を選択 → Use to: do not use
	6. Configure the Logical Volume Manager → <Yes>
	7. Delete logical volume → (どの論理ボリュームを消すか)root, swap, home全て
	8. Display configuration details → Continue
	9. Delete logical volume → user42-vgボリュームが増えているのでこれを選択 → <Yes> (Bonusをやるなら)
	10. Create volume group (Bonusはやらないので/dev/sda5/にuser42-vgグループを再作成) → Go back
	11. Configure encrypted volumes → Create encrypted volumes(sda1しかないことを確認) → 何もせずEnter → Go back
	12. #5を選択 → どうやら使用中で変更できないらしい → <Continue>
	13. Configure the Logical Volume Manager → Delete volume group → 消しちゃう
	14. 戻ってConfigure encrypted volumes → Create encrypted volumes(sda5がふえてる!!) → sda5にチェックを入れてEnter
	15. Done setting up the partitionを選択 → 書き込んじゃっていいか聞かれるので<Yes> → Finish → (sda5のデータランダムなデータ書き込みで消去しちゃうからもう復旧できないけどいいの？って聞かれる) → <Yes>
	16. パスフレーズ8文字入力 → sda5_cryptができた!
	17. Configure the Logical Volume Manager → (変更書き込んで良いか聞かれる)<Yes> → Create volume group → user42-vg → /dev/mapper/sda5-cryptにチェック （Enter）
	18. Create logical volume → user42-vg → 自分の欲しいボリューム名作成（例通りならroot, swap_1, home） → Finish
	19. LVM VG user42-vg, LV root - 2.8 GB Linux device-mapper…などと書かれているものをそれぞれ選択しUse to: を適切なものに変更
		- [home] Ext4 journaling file system → Mount point → /home → Done setting up ...
		- [root] homeと同じ感じで進めていって/を選ぶだけ
		- [swap] swap area
	20. Finish!!! → <Yes>
- [x] Configure Package manager
	0. <No>
	1. Japan
	2. Default
	3. <Continue>
	4. <No>
	5. Default
	6. <Yes> → /dev/sda
- [x] Finish the installation → <Continue> → Debian GNU/Linux
- [x] Login
	0. LVMパスワード8字
	1. user42
	2. pass
- [x] ssh
	0. Network → PortForward New
	1. Guest Address: 10.0.2.15
	2. Guest Port   : 4242
	3. Host Address : 127.0.0.1
	4. Host Port    : 4242
	5. (local)ssh user42@127.0.0.1 -p 4242
	6. <yes>
	7. pass
	8. Finished!!
- [x]


# 仮想マシンをつくったあと
- [x] `sudo`をinstall（`su -`→`apt install sudo`）
	- ［理由］課題指定, `/etc/sudoers`で細かい権限が設定可能, `su`はユーザ切り替えだけど`sudo`は権限切り替えで便利
	- パッケージ管理のシステムの`dpkg`を用いて`sudo`が入ったか確認（`dpkg -l | grep sudo`）
- [x] `sudo`を用いてルートユーザ名とログイン名
- [x] ホスト名を変更する方法
	- ［一時的］`hostname <new_hostname>`
	- ［恒久的］`/etc/hostname`と`/etc/hosts`を確認してホスト名を書き換えてから再起動する
