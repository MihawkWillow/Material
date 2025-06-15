合并两个有序列表
```
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    List<Integer> list = new ArrayList<>();

    while (l1 != null) {
        list.add(l1.val);
        l1 = l1.next;
    }

    while (l2 != null) {
        list.add(l2.val);
        l2 = l2.next;
    }

    // 排序
    Collections.sort(list);

    ListNode vo = new ListNode();
    ListNode nextVo = vo;

    // vo -> nextVo -> nextVo -> nextVo
    //        1        2        3
    // 1 2 3 4 5 6
    for (Integer val : list) {
        nextVo.next = new ListNode(val);
        nextVo = nextVo.next;
    }

    return vo.next;
}  

```

删除有序数组的重复项

![输入图片说明](/imgs/2025-06-09/OXNrSCHlPs0EL8vo.png)

```
public void test() {
    int[] nums = {0, 0, 1, 1, 1, 2, 2, 3, 3, 4};
    // int[] nums = {0, 0, 1, 1, 1, 2, 2, 3, 3, 4};
    // int[] nums = {0, 1, 2, 3, 4, 2, 2, 3, 3, 4};
    System.out.println(removeDuplicates(nums));
}

// 关键字：有序数组 原地删除重复
// 主要操作：去重复（原地）
public int removeDuplicates(int[] nums) {
    // 索引值
    int index = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[index] != nums[i]) {
            nums[++index] = nums[i];
        }
    }
    return ++index;
}

```


> Written with [StackEdit中文版](https://stackedit.cn/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwMDIzOTQ5NTFdfQ==
-->