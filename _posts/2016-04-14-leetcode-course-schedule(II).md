---
layout: post
title: Course Schedule(II)
category: Leetcode
---

* content
{:toc}

## Course Schedule

### 题目描述

> There are a total of n courses you have to take, labeled from `0` to `n - 1`.
>
> Some courses may have prerequisites, for example to take course `0` you have to first take course `1`, which is expressed as a pair: `[0,1]`
>
> Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?
>
> For example:
>
> `2`, `[[1,0]]`
>
> There are a total of `2` courses to take. To take course `1` you should have finished course `0`. So it is possible.
>
> `2`, `[[1,0],[0,1]]`
>
> There are a total of `2` courses to take. To take course `1` you should have finished course `0`, and to take course `0` you should also have finished course `1`. So it is impossible.
>
> Note:
>
> * The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about [how a graph is represented](https://www.khanacademy.org/computing/computer-science/algorithms/graph-representation/a/representing-graphs).

### 解法

代码如下:

    public static boolean canFinish( int numCourses, int[][] prerequisites ) {
        // 邻接矩阵表示图 : matrix[i][j]表示i课程是j课程的先修课程
        int[][] matrix = new int[numCourses][numCourses];
        // 节点入度数组
        int[] indegree = new int[numCourses];

        // 构造邻接矩阵和入度数组
        for( int i = 0; i < prerequisites.length; ++i ) {
            int course = prerequisites[i][0];
            int pre = prerequisites[i][1];
            if( matrix[pre][course] == 0 )
                ++indegree[course];
            matrix[pre][course] = 1;
        }

        // 寻找入度为0的节点, 加入队列
        Queue<Integer> queue = new LinkedList<>();
        for( int i = 0; i < indegree.length; ++i )
            if( indegree[i] == 0 ) queue.offer( i );

        int count = 0;
        while( !queue.isEmpty() ) {
            int course = queue.poll();
            ++count;
            for( int i = 0; i < numCourses; ++i ) {
                if( matrix[course][i] != 0 ) {
                    if( --indegree[i] == 0 )
                        queue.offer( i );
                }
            }
        }
        return count == numCourses;
    }

思考过程: `BFS`. 算法参考自[这里](https://leetcode.com/discuss/35578/easy-bfs-topological-sort-java).

时空复杂度: 时间复杂度是`O(V+E)`, 空间复杂度是`O(V^2)`

- - -

## Course Schedule II

### 题目描述

> There are a total of `n` courses you have to take, labeled from `0` to `n - 1`.
>
> Some courses may have prerequisites, for example to take course `0` you have to first take course `1`, which is expressed as a pair: `[0,1]`
>
> Given the total number of courses and a list of prerequisite pairs, return the ordering of courses you should take to finish all courses.
>
> There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.
>
> For example:
>
> `2`, `[[1,0]]`
>
> There are a total of `2` courses to take. To take course `1` you should have finished course `0`. So the correct course order is `[0,1]`
>
> `4`, `[[1,0],[2,0],[3,1],[3,2]]`
>
> There are a total of 4 courses to take. To take course `3` you should have finished both courses `1` and `2`. Both courses `1` and `2` should be taken after you finished course `0`. So one correct course order is `[0,1,2,3]`. Another correct ordering is`[0,2,1,3]`.

### 解法

代码如下:

    public static int[] findOrder( int numCourses, int[][] prerequisites ) {
        int[] res = new int[numCourses];
        // 矩阵表示图 : matrix.get(i)表示i课程的所有先修课程
        List<List<Integer>> matrix = new ArrayList<>(numCourses);
        for( int i = 0; i < numCourses; ++i ) matrix.add( new ArrayList<>() ); 
        // 节点入度数组
        int[] indegree = new int[numCourses];

        // 构造矩阵和入度数组
        for( int i = 0; i < prerequisites.length; ++i ) {
            int course = prerequisites[i][0];
            int pre = prerequisites[i][1];
            ++indegree[course];
            matrix.get(pre).add(course);
        }

        // 寻找入度为0的节点, 加入队列
        Queue<Integer> queue = new LinkedList<>();
        for( int i = 0; i < indegree.length; ++i )
            if( indegree[i] == 0 ) queue.offer( i );

        int count = 0;
        while( !queue.isEmpty() ) {
            int course = queue.poll();
            res[count++] = course;
            for( int nextCourse : matrix.get(course) ) {
                if( --indegree[nextCourse] == 0 )
                    queue.offer( nextCourse );
            }
        }
        return count == numCourses ? res : new int[0];
    }

思考过程: `BFS`. 算法思想与上题一模一样.

时空复杂度: 时间复杂度是`O(V+E)`, 空间复杂度是`O(V+E)`