### PHP 操作mysql知识练习
> 省略了基本的CRUD，只关注一些算法的实现

#### 分页

```
//根据文章的状态，上架时间，下架时间，关键字模糊查询
//分页
//limit 后面两个参数：偏移位置  偏移量
public function tbGetActList($actListParam, $pageNum, $pageSize) {
        $where = $this->initSearch($actListParam);

        $sql = "select * from " . $this->m_tableName . $where . " ORDER BY creatTime DESC limit " . ($pageNum - 1) * $pageSize . "," . $pageSize;

        $result = array();
        $this->Exec($sql, $result);//PDO 执行
        return $result;
    }

    private function initSearch($actListParam) {
        $where = ' where';
        if (isset($actListParam['status'])) {
            $where .= ' (`status`=\'' . trim($actListParam['status']) . '\')';
        } else {
            $where .= ' (`status` = 0 OR `status` = 1 OR `status` = 2)';
        }
        if (isset($actListParam['addTime']) && !empty($actListParam['addTime'])) {
            $where .= ' AND `addTime` >= \'' . trim($actListParam['addTime']) . '\'';
        }
        if (isset($actListParam['offTime']) && !empty($actListParam['offTime'])) {
            $where .= ' AND `offTime` <= \'' . trim($actListParam['offTime']) . '\'';
        }
        if (isset($actListParam['keyWord']) && !empty($actListParam['keyWord'])) {
            $where .= " AND CONCAT_WS(' ', `gameName`, `title`, `desc`) like '%" . $actListParam['keyWord'] . "%'";
        }
        return $where;

    }
```
