
[Leveraging Deep Visual Descriptors for Hierarchical Efficient Localization](https://arxiv.org/pdf/1809.01019.pdf)

1. keyframes -> distilled VLAD -> global descriptors (prior frames)
2. group prior frames by covisibility, as cluster of places
3. for each places, for query frame, match 3d-2d correspondency

Further reading
- [ ] VLAD, image retrieval
- [ ] distillation

[From Coarse to Fine](https://arxiv.org/pdf/1812.03506.pdf)

1. global, coarse search followed by a fine pose estimation
2. hierarchical features(HF) net, compute local and global feature in one shot

Further reading
- [ ] [P3p](https://rpg.ifi.uzh.ch/docs/CVPR11_kneip.pdf)
- [ ] [SuperPoint](https://arxiv.org/pdf/1712.07629.pdf) more sparser, faster than transposed convolution
- [ ] [LF-Net](https://arxiv.org/pdf/1805.09662.pdf)
- [ ] multitask [distillation](https://arxiv.org/pdf/1503.02531.pdf)

[SuperGlue](https://arxiv.org/pdf/1911.11763.pdf)

Graph neural network for feature matching

Further reading
- [ ] GNN
- [ ] Attention
- [ ] dustbin matching
- [ ] [Sinkhorn Distances](https://proceedings.neurips.cc/paper/2013/file/af21d0c97db2e27e13572cbf59eb343d-Paper.pdf)

[Pixel-Perfect Structure-from-Motion with Featuremetric Refinement](https://arxiv.org/pdf/2108.08291.pdf)
