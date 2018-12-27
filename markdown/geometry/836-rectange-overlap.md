这个题一维二维的情况都很经典，可以记住结论：
* 一维：[l1, r1], [l2, r2]. overlap <=> `l1 < r2 && l2 < r1`
* 二维：[blx1, bly1, trx1, try1], [blx2, bly2, trx2, try2]. overlap <=> `blx1 < trx2 && blx2 < trx1 && bly1 < try2 && bly2 < try1`