My solution comes from the observation that starting from the highest point, and going downwards, either the point is a summit, or it is can be reached by one 
or multiple direct neighbor(s). Hence, the maximum length at that point is the maximum of the maximum lengths that lead to the neighbors if any, or zero, to which 
we add one. When there is equality in the maximums lengths of the neighbors, we simply pick the one which leads to a higher drop. We do this until we reach 
the lowest point of the ski map. 

Pseudo-code:

matrix = n_cols x n_rows skimap matrix
agg = n_cols x n_rows matrix, initialized with None of (l, d)_i,j where l is max_length and d is max_drop for (i, j) point, or None
max_height = max(matrix)
min_height = min(matrix)
current_height = max_height
while current_height >= min_height:
	for point in matrix where height(point) = current_height:
		val = 0
		drop = 0
		for neighbor in neighbors(point): // neighbor(point) returns the neighbors at north, south, west and east when they are still in the ski map
			if height(neighbor) > current_height:
				if max_length(neighbor) > val:
					val = max_length(neighbor)
					drop = drop(neighbor) + (height(neighbor) - current_height)
				elif max_length == val:
					drop = max(drop, drop(neighbor) + (height(neighbor) - current_height))
				else:
					pass
			else:
				pass
		agg[point] = (val + 1, drop)
return max(agg) where max here is taken first on val then (if needed) on drop

Example: 

4 8 7 3 
2 5 9 3 
6 3 2 5 
4 4 1 6

(x, y) where x is the maximum length, and y the maximum drop

height = 9
None None None None
None None (1, 0) None
None None None None
None None None None

height = 8
None (1, 0) None None
None None (1, 0) None
None None None None
None None None None

height = 7
None (1, 0) (2, 2) None
None None (1, 0) None
None None None None
None None None None

height = 6
None (1, 0) (2, 2) None
None None (1, 0) None
(1, 0) None None None
None None None (1, 0)

height = 5
None (1, 0) (2, 2) None
None (2, 4) (1, 0) None
(1, 0) None None (2, 1)
None None None (1, 0)

height = 4
(2, 4) (1, 0) (2, 2) None
None (2, 4) (1, 0) None
(1, 0) None None (2, 1)
(2, 2) (1, 0) None (1, 0)

height = 3
(2, 4) (1, 0) (2, 2) (3, 6)
None (2, 4) (1, 0) (3, 3)
(1, 0) (3, 6) None (2, 1)
(2, 2) (1, 0) None (1, 0)

height = 2
(2, 4) (1, 0) (2, 2) (3, 6)
(3, 7) (2, 4) (1, 0) (3, 3)
(1, 0) (3, 6) (4, 7) (2, 1)
(2, 2) (1, 0) None (1, 0)

height = 2
(2, 4) (1, 0) (2, 2) (3, 6)
(3, 7) (2, 4) (1, 0) (3, 3)
(1, 0) (3, 6) (4, 7) (2, 1)
(2, 2) (1, 0) (5, 8) (1, 0)

Optional:
To find the full path, we start at the coordinates yielding the longest and steepest path and we find at each iteration the neighbor that lead to this solution.

Pseudo-code:
p0 = arg_max(agg) where max here is taken first on val then (if needed) on drop
current_opt = agg[p0]
path = p0
while current_val[0] > 1:
	p = path[-1]
	current_opt = agg[p]
	coord = (,)
	for point in neighbors(p0):
		if agg[point][0] == current_opt[0] - 1 and agg[point][1] == current_opt[1]:
			coord = point
	path.append(coord)
return path
	
