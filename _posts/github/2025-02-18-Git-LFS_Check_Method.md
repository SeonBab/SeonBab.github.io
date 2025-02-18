---
layout: single

title: "GitHub LFS 데이터 확인 방법"

categories:
    - GitHub
tag: [GitHub]

date: 2025-02-18
last_modified_at: 2025-02-18

order : 200000
---

# Git LFS 데이터 확인 방법

사용하고 있는 데이터가 있는 경우 다음과 같습니다.

파일이 원본으로 올라가지 않고, 메타 데이터(포인터)만 리포지토리에 저장되어 다음과 같이 업로드 됩니다.

![LFS_Check_Method-metadata]({{site.url}}/images/GitHub/2025-02-18-Git-LFS_Check_Method/LFS_Check_Method-metadata.PNG)

Git LFS Data를 확인해보면 데이터가 저장된 총 크기를 볼 수 있습니다.

![LFS_Check_Method-Git_LFS_Data]({{site.url}}/images/GitHub/2025-02-18-Git-LFS_Check_Method/LFS_Check_Method-Git_LFS_Data.PNG)

GIt Bash > git lfs ls-files --all  
어떤 파일이 LFS에서 관리되는지 세부적으로 확인이 가능합니다.

![LFS_Check_Method-git_lfs_ls-files]({{site.url}}/images/GitHub/2025-02-18-Git-LFS_Check_Method/LFS_Check_Method-git_lfs_ls-files.PNG)

데이터가 없는 경우 다음과 같이 출력됩니다.

GitUhb 로그인 후 > Settings > Billing and Plans > Plans and usage에서 Git LFS Data 확인

![LFS_Check_Method-Git_LFS_Data_Empty]({{site.url}}/images/GitHub/2025-02-18-Git-LFS_Check_Method/LFS_Check_Method-Git_LFS_Data_Empty.PNG)

GIt Bash > git lfs ls-files --all  
어떤 파일이 LFS에서 관리되는지 확인

![LFS_Check_Method-git_lfs_ls-files_Empty]({{site.url}}/images/GitHub/2025-02-18-Git-LFS_Check_Method/LFS_Check_Method-git_lfs_ls-files_Empty.PNG)

