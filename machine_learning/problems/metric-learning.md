# contrastive loss

# triplet loss

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