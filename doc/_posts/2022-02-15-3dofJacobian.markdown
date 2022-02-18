<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>

# 3Dof 公式推导
## relative Pose


$$
\begin{bmatrix}
e(x) \\
e(y) \\
e(yaw) \\ 
\end{bmatrix} = 
\begin{bmatrix}
x_i \\
y_i \\
yaw_i \\
\end{bmatrix} -
\begin{bmatrix}
x_j \\
y_j \\
yaw_j \\
\end{bmatrix}
$$

$$
J_i = \frac{\delta(e)}{\delta(X_i)} =
\begin{bmatrix}
1 & 0 &0 \\
0 & 1 & 0 \\
0 & 0 & 1 \\
\end{bmatrix}
$$

$$
J_j = \frac{\delta(e)}{\delta(X_j)}=
\begin{bmatrix}
-1 & 0 &0 \\
0 & -1 & 0 \\
0 & 0 & -1 \\
\end{bmatrix}
$$

## 视觉测量误差

### 原始表达形式

$$
P_l^{c_j} = R_b^c( R_w^{b_j}(R^w_{b_i} (R_c^b\frac{P_l^{c_i}}{\lambda_l} + p_c^b) + p^w_{b_i} - p^w_{b_j}) - p_c^b)
$$

### 齐次表达形式

$$
P_l^{c_j} = T_b^c T_w^{b_j} T^w_{b_i} T_c^b\frac{P_l^{c_i}}{\lambda_l} 
$$

$$
P_l^{c_j} = T_w^{c_j} T^w_{b_i} {P_l^{b_i}}
$$

$$
P_l^{c_j} = T_b^c T_w^{b_j} {P_l^w} 
$$

### 重投影误差项

$$
e = 
\frac {P_l^{cj}} {z_j} -
\frac {\bar{P_l^{c_j}}} {\bar{z_j}} 
$$

### 重投影误差项 关于 $P_l^{c_j}$的偏导数

$$
\frac {\partial(e)}{\partial(P_l^{c_j})}
= \begin{bmatrix}
\frac {\partial(e_x)} {\partial(P_lx^{c_j})} & 
\frac {\partial(e_x)} {\partial(P_ly^{c_j})} & 
\frac {\partial(e_x)} {\partial(P_lz^{c_j})} & 
\\
\frac {\partial(e_y)} {\partial(P_lx^{c_j})} & 
\frac {\partial(e_y)} {\partial(P_ly^{c_j})} & 
\frac {\partial(e_y)} {\partial(P_lz^{c_j})} & 
\end{bmatrix}
= \begin{bmatrix}
\frac {1} {z_j} & 
0 & 
-\frac {x_j} {z_j^2} & 
\\
0 &
\frac {1} {z_j} & 
-\frac {y_j} {z_j^2} & 
\end{bmatrix}
$$

### $(P_l^{c_j})$ 关于 $(p^w_{b_i})$的偏导数

$$
\frac{\partial(P_l^{c_j})}{\partial(p_l^{c_i})}
= R_b^c R_w^{b_j} 
$$



### $(P_l^{c_j})$ 关于 $(R^w_{b_i})$的展开公式

$$
P_l^{c_j} = R_w^{c_j} (R^w_{b_i} {P_l^{b_i}} + p^w_{b_i}) + p_w^{c_j} 
\\ = R_w^{c_j} 
\begin{bmatrix}
  x R^w_{b_i11} 
+ y R^w_{b_i12}
+ z R^w_{b_i13}
+ c
\\
  x R^w_{b_i21}
+ y R^w_{b_i22}
+ z R^w_{b_i23}
+ c
\\
  x R^w_{b_i31}
+ y R^w_{b_i32}
+ z R^w_{b_i33}
+ c
\end{bmatrix}
\\
= 

\begin{bmatrix}
R_{w11}^{c_j} 
 (x R^w_{b_i11} + y R^w_{b_i12} + z R^w_{b_i13})
+
R_{w12}^{c_j} 
( x R^w_{b_i21} + y R^w_{b_i22} + z R^w_{b_i23})
+
R_{w13}^{c_j} 
 (x R^w_{b_i31} + y R^w_{b_i32} + z R^w_{b_i33})
+ c
\\
R_{w21}^{c_j} 
 (x R^w_{b_i11} + y R^w_{b_i12} + z R^w_{b_i13})
+
R_{w22}^{c_j} 
( x R^w_{b_i21} + y R^w_{b_i22} + z R^w_{b_i23})
+
R_{w23}^{c_j} 
 (x R^w_{b_i31} + y R^w_{b_i32} + z R^w_{b_i33})
 + c 
\\
R_{w31}^{c_j} 
 (x R^w_{b_i11} + y R^w_{b_i12} + z R^w_{b_i13})
+
R_{w32}^{c_j} 
( x R^w_{b_i21} + y R^w_{b_i22} + z R^w_{b_i23})
+
R_{w33}^{c_j} 
 (x R^w_{b_i31} + y R^w_{b_i32} + z R^w_{b_i33})
 + c 
\end{bmatrix}
$$

### $(P_l^{c_j})$ 关于 $(R^w_{b_i})$的偏导数

$$
\frac{\partial(P_l^{c_j})}{\partial(R^w_{b_i})}
=   \begin{bmatrix}
\frac{\partial(P_lx^{c_j})} {\partial(R^w_{b_i11})} & 
\frac{\partial(P_lx^{c_j})} {\partial(R^w_{b_i12})} & 
\frac{\partial(P_lx^{c_j})} {\partial(R^w_{b_i13})} &
\frac{\partial(P_lx^{c_j})} {\partial(R^w_{b_i21})} &
\frac{\partial(P_lx^{c_j})} {\partial(R^w_{b_i22})} &
\frac{\partial(P_lx^{c_j})} {\partial(R^w_{b_i23})} &
\frac{\partial(P_lx^{c_j})} {\partial(R^w_{b_i31})} &
\frac{\partial(P_lx^{c_j})} {\partial(R^w_{b_i32})} &
\frac{\partial(P_lx^{c_j})} {\partial(R^w_{b_i33})} &

\\

\frac{\partial(P_ly^{c_j})} {\partial(R^w_{b_i11})} & 
\frac{\partial(P_ly^{c_j})} {\partial(R^w_{b_i12})} & 
\frac{\partial(P_ly^{c_j})} {\partial(R^w_{b_i13})} &
\frac{\partial(P_ly^{c_j})} {\partial(R^w_{b_i21})} &
\frac{\partial(P_ly^{c_j})} {\partial(R^w_{b_i22})} &
\frac{\partial(P_ly^{c_j})} {\partial(R^w_{b_i23})} &
\frac{\partial(P_ly^{c_j})} {\partial(R^w_{b_i31})} &
\frac{\partial(P_ly^{c_j})} {\partial(R^w_{b_i32})} &
\frac{\partial(P_ly^{c_j})} {\partial(R^w_{b_i33})} &
\\

\frac{\partial(P_lz^{c_j})} {\partial(R^w_{b_i11})} & 
\frac{\partial(P_lz^{c_j})} {\partial(R^w_{b_i12})} & 
\frac{\partial(P_lz^{c_j})} {\partial(R^w_{b_i13})} &
\frac{\partial(P_lz^{c_j})} {\partial(R^w_{b_i21})} &
\frac{\partial(P_lz^{c_j})} {\partial(R^w_{b_i22})} &
\frac{\partial(P_lz^{c_j})} {\partial(R^w_{b_i23})} &
\frac{\partial(P_lz^{c_j})} {\partial(R^w_{b_i31})} &
\frac{\partial(P_lz^{c_j})} {\partial(R^w_{b_i32})} &
\frac{\partial(P_lz^{c_j})} {\partial(R^w_{b_i33})} &
\\
\end{bmatrix}
\\ =
\begin{bmatrix}
  R_{w11}^{c_j} x & R_{w11}^{c_j}y &  R_{w11}^{c_j}z 
& R_{w12}^{c_j} x & R_{w12}^{c_j}y &  R_{w12}^{c_j}z 
& R_{w13}^{c_j} x & R_{w13}^{c_j}y &  R_{w13}^{c_j}z 
\\R_{w21}^{c_j} x & R_{w21}^{c_j}y &  R_{w21}^{c_j}z 
& R_{w22}^{c_j} x & R_{w22}^{c_j}y &  R_{w22}^{c_j}z 
& R_{w23}^{c_j} x & R_{w23}^{c_j}y &  R_{w23}^{c_j}z 
\\R_{w31}^{c_j} x & R_{w31}^{c_j}y &  R_{w31}^{c_j}z 
& R_{w32}^{c_j} x & R_{w32}^{c_j}y &  R_{w32}^{c_j}z 
& R_{w33}^{c_j} x & R_{w33}^{c_j}y &  R_{w33}^{c_j}z 
\end{bmatrix}
$$

$$
\frac{\partial(P_l^{c_j})}{\partial(R^w_{b_i})}
=
\begin{bmatrix}
\begin{array}{c|c|c}
R^{c_j}_{w col1}[x, y, z]
&
R^{c_j}_{w col2}[x, y, z]
&
R^{c_j}_{w col3}[x, y, z]
\end{array}
\end{bmatrix}
\\
其中
[x, y, z]^T = P^{b_i}_l
$$

设$X=(x,y,\theta)$
### 3Dof 关于$X^w_{b_i}$的雅克比公式

$$
\frac{\partial(e)}{\partial(X^w_{b_i})}
=
\begin{bmatrix}
\frac{\partial(e)}{\partial(x^w_{b_i})}
&
\frac{\partial(e)}{\partial(y^w_{b_i})}
&
\frac{\partial(e)}{\partial(\theta^w_{b_i})}
\end{bmatrix}
=
\begin{bmatrix}
\frac {\partial(e)}{\partial(P_l^{c_j})}
\cdot
\frac {\partial(P_l^{c_j})} {\partial(x^w_{b_i})}
&
\frac {\partial(e)}{\partial(P_l^{c_j})}
\cdot
\frac {\partial(P_l^{c_j})} {\partial(y^w_{b_i})}
&
\frac {\partial(e)}{\partial(P_l^{c_j})}
\cdot
\frac {\partial(P_l^{c_j})} {\partial(R^w_{b_i})}
\cdot
\frac {\partial(R^w_{b_i})} {\partial(\theta)}
\end{bmatrix}
$$

### 3Dof 关于$X^w_{b_j}$的雅克比公式


### $(P_l^{c_j})$ 关于 $(p^w_{b_j})$的偏导数

$$
\frac{\partial(P_l^{c_j})}{\partial(p_w^{b_j})}
= - R_b^c R_w^{b_j} 
$$


### $(P_l^{c_j})$ 关于 $(R_w^{b_j})$的展开公式

$$
P_l^{c} = T_b^c T_w^{b_j} {P_l^w} 
$$

$$
P_l^{c} = R_b^{c} R_w^{b_j} ({P_l^{w}} - p^w_{b_j}) + p^c_{b}

\\ = R_b^{c} 
\begin{bmatrix}
  x R^{b_j}_{w11} 
+ y R^{b_j}_{w12}
+ z R^{b_j}_{w13}
+ c
\\
  x R^{b_j}_{w21}
+ y R^{b_j}_{w22}
+ z R^{b_j}_{w23}
+ c
\\
  x R^{b_j}_{w31}
+ y R^{b_j}_{w32}
+ z R^{b_j}_{w33}
+ c
\end{bmatrix}
\\
= 

\begin{bmatrix}
R_{b11}^{c} 
 (x R^{b_j}_{w11} + y R^{b_j}_{w12} + z R^{b_j}_{w13})
+
R_{b12}^{c} 
( x R^{b_j}_{w21} + y R^{b_j}_{w22} + z R^{b_j}_{w23})
+
R_{b13}^{c} 
 (x R^{b_j}_{w31} + y R^{b_j}_{w32} + z R^{b_j}_{w33})
+ c
\\
R_{b21}^{c} 
 (x R^{b_j}_{w11} + y R^{b_j}_{w12} + z R^{b_j}_{w13})
+
R_{b22}^{c} 
( x R^{b_j}_{w21} + y R^{b_j}_{w22} + z R^{b_j}_{w23})
+
R_{b23}^{c} 
 (x R^{b_j}_{w31} + y R^{b_j}_{w32} + z R^{b_j}_{w33})
 + c 
\\
R_{b31}^{c} 
 (x R^{b_j}_{w11} + y R^{b_j}_{w12} + z R^{b_j}_{w13})
+
R_{b32}^{c} 
( x R^{b_j}_{w21} + y R^{b_j}_{w22} + z R^{b_j}_{w23})
+
R_{b33}^{c} 
 (x R^{b_j}_{w31} + y R^{b_j}_{w32} + z R^{b_j}_{w33})
 + c 
\end{bmatrix}
$$

### $(P_l^{c})$ 关于 $(R_w^{b_j})$的偏导数

$$
\frac{\partial(P_l^{c})}{\partial(R^{b_j}_{w})}
=   \begin{bmatrix}
\frac{\partial(P_lx^{c})} {\partial(R^{b_j}_{w11})} & 
\frac{\partial(P_lx^{c})} {\partial(R^{b_j}_{w12})} & 
\frac{\partial(P_lx^{c})} {\partial(R^{b_j}_{w13})} &
\frac{\partial(P_lx^{c})} {\partial(R^{b_j}_{w21})} &
\frac{\partial(P_lx^{c})} {\partial(R^{b_j}_{w22})} &
\frac{\partial(P_lx^{c})} {\partial(R^{b_j}_{w23})} &
\frac{\partial(P_lx^{c})} {\partial(R^{b_j}_{w31})} &
\frac{\partial(P_lx^{c})} {\partial(R^{b_j}_{w32})} &
\frac{\partial(P_lx^{c})} {\partial(R^{b_j}_{w33})} &

\\

\frac{\partial(P_ly^{c})} {\partial(R^{b_j}_{w11})} & 
\frac{\partial(P_ly^{c})} {\partial(R^{b_j}_{w12})} & 
\frac{\partial(P_ly^{c})} {\partial(R^{b_j}_{w13})} &
\frac{\partial(P_ly^{c})} {\partial(R^{b_j}_{w21})} &
\frac{\partial(P_ly^{c})} {\partial(R^{b_j}_{w22})} &
\frac{\partial(P_ly^{c})} {\partial(R^{b_j}_{w23})} &
\frac{\partial(P_ly^{c})} {\partial(R^{b_j}_{w31})} &
\frac{\partial(P_ly^{c})} {\partial(R^{b_j}_{w32})} &
\frac{\partial(P_ly^{c})} {\partial(R^{b_j}_{w33})} &
\\

\frac{\partial(P_lz^{c})} {\partial(R^{b_j}_{w11})} & 
\frac{\partial(P_lz^{c})} {\partial(R^{b_j}_{w12})} & 
\frac{\partial(P_lz^{c})} {\partial(R^{b_j}_{w13})} &
\frac{\partial(P_lz^{c})} {\partial(R^{b_j}_{w21})} &
\frac{\partial(P_lz^{c})} {\partial(R^{b_j}_{w22})} &
\frac{\partial(P_lz^{c})} {\partial(R^{b_j}_{w23})} &
\frac{\partial(P_lz^{c})} {\partial(R^{b_j}_{w31})} &
\frac{\partial(P_lz^{c})} {\partial(R^{b_j}_{w32})} &
\frac{\partial(P_lz^{c})} {\partial(R^{b_j}_{w33})} &
\\
\end{bmatrix}
\\ =

\begin{bmatrix}
  R_{b11}^{c} x & R_{b11}^{c}y &  R_{b11}^{c}z 
& R_{b12}^{c} x & R_{b12}^{c}y &  R_{b12}^{c}z 
& R_{b13}^{c} x & R_{b13}^{c}y &  R_{b13}^{c}z 
\\R_{b21}^{c} x & R_{b21}^{c}y &  R_{b21}^{c}z 
& R_{b22}^{c} x & R_{b22}^{c}y &  R_{b22}^{c}z 
& R_{b23}^{c} x & R_{b23}^{c}y &  R_{b23}^{c}z 
\\R_{b31}^{c} x & R_{b31}^{c}y &  R_{b31}^{c}z 
& R_{b32}^{c} x & R_{b32}^{c}y &  R_{b32}^{c}z 
& R_{b33}^{c} x & R_{b33}^{c}y &  R_{b33}^{c}z 
\end{bmatrix}
$$


### $(P_l^{c})$ 关于 $(R^w_{b_j})$的偏导数

$$
\frac{\partial(P_l^{c})}{\partial(R^{w}_{b_j})}
=   \begin{bmatrix}
\frac{\partial(P_lx^{c})} {\partial(R^{w}_{b_j11})} & 
\frac{\partial(P_lx^{c})} {\partial(R^{w}_{b_j12})} & 
\frac{\partial(P_lx^{c})} {\partial(R^{w}_{b_j13})} &
\frac{\partial(P_lx^{c})} {\partial(R^{w}_{b_j21})} &
\frac{\partial(P_lx^{c})} {\partial(R^{w}_{b_j22})} &
\frac{\partial(P_lx^{c})} {\partial(R^{w}_{b_j23})} &
\frac{\partial(P_lx^{c})} {\partial(R^{w}_{b_j31})} &
\frac{\partial(P_lx^{c})} {\partial(R^{w}_{b_j32})} &
\frac{\partial(P_lx^{c})} {\partial(R^{w}_{b_j33})} &

\\

\frac{\partial(P_ly^{c})} {\partial(R^{w}_{b_j11})} & 
\frac{\partial(P_ly^{c})} {\partial(R^{w}_{b_j12})} & 
\frac{\partial(P_ly^{c})} {\partial(R^{w}_{b_j13})} &
\frac{\partial(P_ly^{c})} {\partial(R^{w}_{b_j21})} &
\frac{\partial(P_ly^{c})} {\partial(R^{w}_{b_j22})} &
\frac{\partial(P_ly^{c})} {\partial(R^{w}_{b_j23})} &
\frac{\partial(P_ly^{c})} {\partial(R^{w}_{b_j31})} &
\frac{\partial(P_ly^{c})} {\partial(R^{w}_{b_j32})} &
\frac{\partial(P_ly^{c})} {\partial(R^{w}_{b_j33})} &
\\

\frac{\partial(P_lz^{c})} {\partial(R^{w}_{b_j11})} & 
\frac{\partial(P_lz^{c})} {\partial(R^{w}_{b_j12})} & 
\frac{\partial(P_lz^{c})} {\partial(R^{w}_{b_j13})} &
\frac{\partial(P_lz^{c})} {\partial(R^{w}_{b_j21})} &
\frac{\partial(P_lz^{c})} {\partial(R^{w}_{b_j22})} &
\frac{\partial(P_lz^{c})} {\partial(R^{w}_{b_j23})} &
\frac{\partial(P_lz^{c})} {\partial(R^{w}_{b_j31})} &
\frac{\partial(P_lz^{c})} {\partial(R^{w}_{b_j32})} &
\frac{\partial(P_lz^{c})} {\partial(R^{w}_{b_j33})} &
\\
\end{bmatrix}
\\ =
\begin{bmatrix}
\frac{\partial(P_lx^{c})} {\partial(R^{b_j}_{w11})} & 
\frac{\partial(P_lx^{c})} {\partial(R^{b_j}_{w21})} &
\frac{\partial(P_lx^{c})} {\partial(R^{b_j}_{w31})} &
\frac{\partial(P_lx^{c})} {\partial(R^{b_j}_{w12})} & 
\frac{\partial(P_lx^{c})} {\partial(R^{b_j}_{w22})} &
\frac{\partial(P_lx^{c})} {\partial(R^{b_j}_{w32})} &
\frac{\partial(P_lx^{c})} {\partial(R^{b_j}_{w13})} &
\frac{\partial(P_lx^{c})} {\partial(R^{b_j}_{w23})} &
\frac{\partial(P_lx^{c})} {\partial(R^{b_j}_{w33})} &

\\

\frac{\partial(P_ly^{c})} {\partial(R^{b_j}_{w11})} & 
\frac{\partial(P_ly^{c})} {\partial(R^{b_j}_{w21})} &
\frac{\partial(P_ly^{c})} {\partial(R^{b_j}_{w31})} &

\frac{\partial(P_ly^{c})} {\partial(R^{b_j}_{w12})} & 
\frac{\partial(P_ly^{c})} {\partial(R^{b_j}_{w22})} &
\frac{\partial(P_ly^{c})} {\partial(R^{b_j}_{w32})} &

\frac{\partial(P_ly^{c})} {\partial(R^{b_j}_{w13})} &
\frac{\partial(P_ly^{c})} {\partial(R^{b_j}_{w23})} &
\frac{\partial(P_ly^{c})} {\partial(R^{b_j}_{w33})} &
\\

\frac{\partial(P_lz^{c})} {\partial(R^{b_j}_{w11})} & 
\frac{\partial(P_lz^{c})} {\partial(R^{b_j}_{w21})} &
\frac{\partial(P_lz^{c})} {\partial(R^{b_j}_{w31})} &

\frac{\partial(P_lz^{c})} {\partial(R^{b_j}_{w12})} & 
\frac{\partial(P_lz^{c})} {\partial(R^{b_j}_{w22})} &
\frac{\partial(P_lz^{c})} {\partial(R^{b_j}_{w32})} &

\frac{\partial(P_lz^{c})} {\partial(R^{b_j}_{w13})} &
\frac{\partial(P_lz^{c})} {\partial(R^{b_j}_{w23})} &
\frac{\partial(P_lz^{c})} {\partial(R^{b_j}_{w33})} &
\\
\end{bmatrix}
\\ =
\begin{bmatrix}
  R_{b11}^{c} x & R_{b12}^{c} x & R_{b13}^{c} x & R_{b11}^{c}y   & R_{b12}^{c}y & R_{b13}^{c}y &  R_{b11}^{c}z  &  R_{b12}^{c}z &  R_{b13}^{c}z 
\\R_{b21}^{c} x & R_{b22}^{c} x & R_{b23}^{c} x & R_{b21}^{c}y   & R_{b22}^{c}y & R_{b23}^{c}y &  R_{b21}^{c}z  &  R_{b22}^{c}z &  R_{b23}^{c}z 
\\R_{b31}^{c} x & R_{b32}^{c} x & R_{b33}^{c} x & R_{b31}^{c}y   & R_{b32}^{c}y & R_{b33}^{c}y &  R_{b31}^{c}z  &  R_{b32}^{c}z &  R_{b33}^{c}z 
\end{bmatrix}
$$

$$
\frac{\partial(P_l^{c})}{\partial(R^{w}_{b_j})}
=
\begin{bmatrix}
\begin{array}{c|c|c}
xR_b^c & yR_b^c & zR_b^c 
\end{array}
\end{bmatrix}
\\
其中
[x, y, z]^T = P^w_{l} - p^w_{bj}
$$

### 3Dof 关于$X^w_{b_j}$的雅克比公式

$$
\frac{\partial(e)}{\partial(X^w_{b_j})}
=
\begin{bmatrix}
\frac{\partial(e)}{\partial(x^w_{b_j})}
&
\frac{\partial(e)}{\partial(y^w_{b_j})}
&
\frac{\partial(e)}{\partial(\theta^w_{b_j})}
\end{bmatrix}
=
\begin{bmatrix}
\frac {\partial(e)}{\partial(P_l^{c_j})}
\cdot
\frac {\partial(P_l^{c_j})} {\partial(x^w_{b_j})}
&
\frac {\partial(e)}{\partial(P_l^{c_j})}
\cdot
\frac {\partial(P_l^{c_j})} {\partial(y^w_{b_j})}
&
\frac {\partial(e)}{\partial(P_l^{c_j})}
\cdot
\frac {\partial(P_l^{c_j})} {\partial(R^w_{b_j})}
\cdot
\frac {\partial(R^w_{b_j})} {\partial(\theta)}
\end{bmatrix}
$$

### 3Dof 关于$\lambda_l$的雅克比公式


### 原始表达形式

$$
\frac{\partial(P_l^{c_j} )}
{\partial{\lambda_l}}
= R_b^c R_w^{b_j} R^w_{b_i} R_c^b
\frac{P_l^{c_i}}
{-\lambda_l^2}
$$
