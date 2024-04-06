## rm (Remove)

- 파일이나 디렉토리를 삭제
- 한번 지운 파일은 복구가 어렵기 때문에 rm 명령어를 사용하기 전에 확인 필요
- 한번 삭제한 파일에 대한 취소 명령어는 없으므로 신죽하게 삭제 필요
- 비어있지 않은 디렉토리는 -r 옵션 없이는 삭제할 수 없음


```bash
> rm [옵션] [삭제할 디렉토리 or 파일]

# hello.txt 삭제
rm hello.txt

# /home/usr/hello.txt 삭제
rm /home/usr/hello.txt

# hello_folder 디렉토리 삭제
rm -r hello_folder

# hello_folder 디렉토리 삭제 시 삭제 확인 메시지를 출력하지 않음 (묻지 않고 바로 삭제)
rm -rf hello_folder
```
### 주요 옵션

- **-f** : 강제로 파일이나 디렉토리를 삭제하고 대상이 없는 경우에는 메시지를 출력하지 않음
- **-r** : 디렉토리 내부의 모든 내용을 삭제
- **-d** : 비어있는 디렉토리들만 제거
- **-i** : 매번 삭제할 때마다 사용자에게 삭제할 것인지 물음
- **-l** : 3개의 이상의 파일을 삭제하거나 디렉토리 내부가 비어있지 않을때만 삭제할 것인지 물음
- **-v** : 삭제되는 대상의 정보를 출력

---

## mv (Move)

- 파일이나 디렉토리를 이동하거나 이름을 변경하는 명령어
- 경로를 지정하지 않으면 현 위치를 기본으로 함
- 현재 위치에 기존 있는 파일을 이름만 바꿔 이동시켜 이름바꾸기로 응용할 수 있음



```bash
mv [옵션] [이동할 파일 or 디렉토리] [이동할 경로]

# 현재 디렉토리에 있는 파일을 디렉토리 내부의 folder 디렉토리로 이동
mv file.txt folderName

# 현재 디렉토리에 있는 test.txt 파일을 new_test.txt라는 파일로 이름 바꾸기
mv test.txt new_test.txt

# /home/user 경로의 test.txt 파일을 /home/var 디렉토리로 이동
mv /home/user/test.txt /home/var

# /home/user 경로 test.txt 파일을 /home/var 디렉토리에 new.txt로 바꾸어 이동
mv /home/user/test.txt /home/var/new.txt

```

### 주요 옵션
- **-f** : (force) 대상 파일이나 디렉토리가 존재할 경우 덮어쓰기
- **-n** : (no-clobber) 대상 파일이나 디렉토리가 존재할 경우 덮어쓰지 않음
- **-i** : (interactive) 대상 파일이나 디렉토리가 존재할 경우 덮어쓰기 여부를 물음
- **-b** : (backup) 대상 파일이나 디렉토리가 존재할 경우 백업 파일을 만들고 덮어쓰기
- **-u** : (update) 대상 파일이나 디렉토리가 존재할 경우 최신 파일로 덮어쓰기
- **-v** : (verbose) 파일이나 디렉토리를 이동할 때 이동되는 대상의 정보를 출력