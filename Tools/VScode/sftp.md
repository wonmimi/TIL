#### Error: No such File 

파일 업로드를 하는데 갑자기 뜬 에러 메세지.
서버 업로드엔 지장이 없었음.

![이미지 파일](../../img/vscode_error.png)

```bash
[05-14 11:06:52] [error] Error: No such file
	at SFTPStream._transform (/Users/biz_team2006/.vscode/extensions/liximomo.sftp-1.12.9/node_modules/ssh2-streams/lib/sftp.js:412:27)
	at SFTPStream.Transform._read (internal/streams/transform.js:205:10)
	at SFTPStream._read (/Users/biz_team2006/.vscode/extensions/liximomo.sftp-1.12.9/node_modules/ssh2-streams/lib/sftp.js:183:15)
	at SFTPStream.Transform._write (internal/streams/transform.js:193:12)
	at writeOrBuffer (internal/streams/writable.js:358:12)
```

로그에 찍혀있는 
~/.vscode/extensions/liximomo.sftp-1.12.9/node_modules/ssh2-streams/lib/sftp.js  해당 파일에서 

389번 라인을 수정 하였더니 해결 ! 
```javascript
if (code === STATUS_CODE.OK || code === STATUS_CODE.NO_SUCH_FILE) {
      cb();
}
```

vscode가 버전업 되며 에러가 호출되는 현상이 나타난것으로 보여 다음 업데이트를 기대하는게 좋을듯 
[참고](https://github.com/liximomo/vscode-sftp/issues/919)
