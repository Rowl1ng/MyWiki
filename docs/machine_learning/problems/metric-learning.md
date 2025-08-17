# contrastive loss

# triplet loss

  * triplet loss为什么通常L2 normalize：1.避免只是对embedding做scaling 2.确定了L2 distance范围，方便选择margin
  
 In triplet loss training, a triplet contains two images belonging to the same
class, referred to as the anchor and positive samples, and
a third image, from a different class, which is referred to
as the negative sample. The triplet loss function is given
as, $[d(a, p) - d(a, n) + m]+$, where $a$, $p$ and $n$ are anchor,
positive, and negative samples, respectively. $d(\cdot,\cdot)$ is the
learned metric function and $m$ is a margin term which encourages the negative sample to be further from the anchor
than the positive sample.

# NormSoftmax loss


# Sampling