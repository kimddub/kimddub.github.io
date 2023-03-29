
썸머노트(summernote)
===========

# 썸머노트 적용

## 이슈
썸머노트를 적용하면서 발생했던 이슈 종류와 해결방안 정리

1. 전체 텍스트 글꼴 및 사이즈 변경 버그  
   - 원인
     - 붙여넣은 텍스트 더미를 전체 드래그하여 글꼴이나 사이즈 수정 시 앞부분만 적용되는 현상
   - 해결방안 #1
     - summernote-lite를 적용하였는데, 잔버그가 많다하여 summernote-bs4 적용하였음에도 동일 증상 있음
   - 해결방안 #2
     - dom.js 수정
     - [summernote github](https://github.com/summernote/summernote/issues/4040) 참고