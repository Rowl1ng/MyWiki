How to Write a Great Research Paper, and Get it Accepted by a Good Journal: 

- good content(novel,clear,useful, exciting) and presentation(logical manner)
- reviewers can grasp the scientific significance easily, save their time!think from the reader; journal Finder
- clarity, objectivity, accuracy, and brevity(direct and short sentences): tenses; scopus

# Abstract

avoid mentioning new concepts if reader can't perceive it immediately.


# Figures

- Tools: InkScape
- Format: pdf

# Reference Format

check references formats: 

- use abbreviations of conf./journal which need to be consistent throughout the references.
- try to make reference clean: remove volumn, page, publisher, institution.

[Reference][1]
```
@inproceedings{chen2021SimSiam,
    title={Exploring simple {S}iamese representation learning},
    author={Chen, Xinlei and He, Kaiming},
    booktitle=cvpr,
    year={2021}
}

@inproceedings{cubuk2019randaugment,
    title={RandAugment: Practical automated data augmentation with a reduced search space},
    author={Cubuk, Ekin D and Zoph, Barret and Shlens, Jonathon and Le, Quoc V},
    booktitle=cvprw,
    year={2020}
}

@article{becker1992self,
    title={Self-organizing neural network that discovers surfaces in random-dot stereograms},
    author={Becker, Suzanna and Hinton, Geoffrey E},
    journal={Nature},
    year={1992}
}

  @article{ba2016layer,
      title={Layer normalization},
      author={Ba, Jimmy Lei and Kiros, Jamie Ryan and Hinton, Geoffrey E},
      journal={arXiv:1607.06450},
      year={2016}
  }

  @misc{wu2019detectron2,
      author={Yuxin Wu and Alexander Kirillov and Francisco Massa and Wan-Yen Lo and Ross Girshick},
      title={Detectron2},
      howpublished={\url{https://github.com/facebookresearch/detectron2}},
      year={2019}
  }

  @article{tian2019contrastive,
      title={Contrastive multiview coding},
      author={Tian, Yonglong and Krishnan, Dilip and Isola, Phillip},
      journal={arXiv:1906.05849},
      year={2019},
      note={Updated version accessed at \url{https://openreview.net/pdf?id=BkgStySKPB}}
  }
```

# Review pipeline


- review the main text: read it to check the clarity and fluency.
- review the tables/graphs.
    - Are they cited in the text? Are they cited with correct index?
- check if the pdf is submitted in the submission system.



# Tools

## Overleaf

If there's format bug, can downgrade Tex Live version from 2021 to 2020.

## Texpad

Texpad live 支持实时编译
缺点：不支持高级packages，如cref, 会报错：infinite loop in pdfLaTeX code
```
\usepackage[capitalize]{cleveref}
%\crefname{section}{Sec.}{Secs.}
%\Crefname{section}{Section}{Sections}
%\Crefname{table}{Table}{Tables}
%\crefname{table}{Tab.}{Tabs.}
```
需要最后再调整。

[1]:https://xinntao.github.io/records/%E6%B5%81%E7%A8%8B%E8%A7%84%E8%8C%83%E4%B8%8E%E7%BB%8F%E9%AA%8C/10-%E6%96%87%E7%8C%AE%E6%A0%BC%E5%BC%8F%E4%B8%8E%E7%AE%A1%E7%90%86.html