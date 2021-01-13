# No.40 MySQL：外键约束（CASCADE，RESTRICT，NO ACTION）

## 导读

> 弄懂数据库的外键约束。

MySQL有两种常用的引擎类型：MyISAM和InnoDB。目前只有InnoDB引擎类型支持外键约束。

外键的使用需要满足下列的条件：

- 两张表必须都是InnoDB表，并且它们没有临时表。

- 建立外键关系的对应列必须具有相似的InnoDB内部数据类型。

- 建立外键关系的对应列必须建立了索引。

- 假如显式的给出了CONSTRAINT symbol，那symbol在数据库中必须是唯一的。假如没有显式的给出，InnoDB会自动的创建。

如果子表试图创建一个在父表中不存在的外键值，InnoDB会拒绝任何INSERT或UPDATE操作。

如果父表试图UPDATE或者DELETE任何子表中存在或匹配的外键值，最终动作取决于外键约束定义中的ON UPDATE和ON DELETE选项。

InnoDB支持5种不同的动作，如果没有指定ON DELETE或者ON UPDATE，默认的动作为RESTRICT:

- CASCADE: 从父表中删除或更新对应的行，同时自动的删除或更新自表中匹配的行。ON DELETE CANSCADE和ON UPDATE CANSCADE都被InnoDB所支持。

- SET NULL: 从父表中删除或更新对应的行，同时将子表中的外键列设为空。注意，这些在外键列没有被设为NOT NULL时才有效。ON DELETE SET NULL和ON UPDATE SET SET NULL都被InnoDB所支持。

- NO ACTION: InnoDB拒绝删除或者更新父表。

- RESTRICT: 拒绝删除或者更新父表。指定RESTRICT（或者NO ACTION）和忽略ON DELETE或者ON UPDATE选项的效果是一样的。

- SET DEFAULT: InnoDB目前不支持。

外键约束使用最多的两种情况无外乎：

- 父表更新时子表也更新，父表删除时如果子表有匹配的项，删除失败；

- 父表更新时子表也更新，父表删除时子表匹配的项也删除。

前一种情况，在外键定义中，我们使用ON UPDATE CASCADE ON DELETE RESTRICT；后一种情况，可以使用ON UPDATE CASCADE ON DELETE CASCADE。

InnoDB允许你使用ALTER TABLE在一个已经存在的表上增加一个新的外键。

InnoDB也支持使用ALTER TABLE来删除外键。
