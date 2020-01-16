---
layout: post
title: VI Editor Cheat Sheet
categories: Dev
---

## VI Editor 명령어 시트



주의사항:

VI 는 대소문자에 유의해야 합니다.

VI는 insertion mode 와 command mode의 2가지 모드가 있습니다.

ESC를 누르면 command mode로 돌아옵니다.



| 명령어      |                                             |
| ----------- | ------------------------------------------- |
| vi filename | Edits filename                              |
| :w          | Saves current file but no exit              |
| :q          | Quits VI and may prompt if you need to save |
| :wq         | Saves and Quits                             |
| i           | Insert before cursor                        |
| a           | Append after cursor                         |
| o           | open a new line after current line          |
| r           | replace one character                       |
| h           | Move left                                   |
| j           | Move down                                   |
| k           | Move up                                     |
| l           | Move right                                  |
| x           | Delete charater to the right of cursor      |
| dd          | Delete current line                         |

