# Tools

* 弄好了latex+sublime+skim的环境

## Overleaf

If there's format bug, can downgrade Tex Live version from 2021 to 2020.

* 用Overleaf的git支持本地用Texpad编辑论文：不支持cref，需要最后调整
  
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

# Maths


- \mathcal 花体： $$\mathcal X$$
- \mathbf 粗体: $$\mathbf y$$


# 图片
控制大小+图片居中
```
\begin{figure*}
% \begin{overpic}
\centering
\includegraphics[scale=0.8]{figure/mass.jpg}
% \end{overpic}
\caption{钼钯中的肿块：\textbf{a} 边缘平滑、形状规则的良性肿块；\textbf{b} 边缘模糊、形状不规则的恶性肿块}
\label{fig:mass_type}
\end{figure*}
```

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