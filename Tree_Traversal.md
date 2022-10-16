

## Pre, In, Post Order



### Pre Order

```Java 
// Pre Order Non recursion

// stack reverse: root left right
public static void preOrderUnRecur(Node head) {
	System.out.print("pre-order: ");
	if (head != null) {
		Stack<Node> stack = new Stack<Node>();
		stack.add(head);
		while (!stack.isEmpty()) {
			head = stack.pop(); // 弹出栈顶元素
			System.out.print(head.value + " ");
			if (head.right != null) { // 左子树进栈
				stack.push(head.right);
			}
			if (head.left != null) { // 右子树进栈
				stack.push(head.left);
			}
		}
	}
	System.out.println();
}

```



### In Order

```java
public static void inOrderUnRecur(Node head) {
	System.out.print("in-order: ");
	if (head != null) {
		Stack<Node> stack = new Stack<Node>();
		while (!stack.isEmpty() || head != null) {
			if (head != null) { // 如果向左没走到头，则不断向左走，并将遍历过的元素入栈
				stack.push(head);
				head = head.left;
			} else { // 如果向左走到头了，则出栈一个元素，然后将其右孩子入栈
				head = stack.pop();
				System.out.print(head.value + " ");
				head = head.right;
			}
		}
	}
	System.out.println();
}

```



### Post Order

```java
public static void posOrderUnRecur1(Node head) {
	System.out.print("pos-order: ");
	if (head != null) {
		Stack<Node> s1 = new Stack<Node>();
		Stack<Node> s2 = new Stack<Node>();
		s1.push(head);
		while (!s1.isEmpty()) {
			head = s1.pop();
			s2.push(head); // 每一个从 s1 中弹出的节点都放进 s2 中
			if (head.left != null) {
				s1.push(head.left); // 将 cur 的左孩子节点压入 s1 中
			}
			if (head.right != null) {
				s1.push(head.right); // 将 cur 的右孩子节点压入 s1 中
			}
		}
		while (!s2.isEmpty()) { // 从 s2 中依次弹出节点并打印
			System.out.print(s2.pop().value + " ");
		}
	}
	System.out.println();
}

```

