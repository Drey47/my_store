## For creating : produit_scalaire_interpretation.png
```` Python
# Redéfinir les vecteurs pour que le cas du milieu soit bien un angle obtus (et non un angle plat)
vectors_corrected = [
    (np.array([2, 1]), np.array([1, 2])),    # angle aigu, produit scalaire > 0
    (np.array([2, 1]), np.array([-2, 1])),   # angle obtus, produit scalaire < 0
    (np.array([1, 0]), np.array([0, 2]))     # angle droit, produit scalaire = 0
]

titles_corrected = [
    "Produit scalaire > 0 (angle aigu)",
    "Produit scalaire < 0 (angle obtus)",
    "Produit scalaire = 0 (angle droit)"
]

fig, axes = plt.subplots(1, 3, figsize=(15, 5))

for ax, (u, v), title in zip(axes, vectors_corrected, titles_corrected):
    ax.quiver(0, 0, *u, angles='xy', scale_units='xy', scale=1, color='b', label='u')
    ax.quiver(0, 0, *v, angles='xy', scale_units='xy', scale=1, color='g', label='v')
    
    dot_product = np.dot(u, v)
    ax.set_title(f"{title}\nProduit scalaire = {dot_product}")
    ax.set_xlim(-3, 3)
    ax.set_ylim(-3, 3)
    ax.grid(True)
    ax.set_aspect('equal')
    ax.legend()

plt.tight_layout()
#plt.show()
plt.savefig("produit_scalaire_interpretation.png")

````


###### **Étape 1 : Calcul du Déterminant**  
Le déterminant d’une matrice $ 3 \times 3 $ :  
$
A = \begin{pmatrix}
a & b & c \\
d & e & f \\
g & h & i
\end{pmatrix}
$  
est donné par :  
$
\det(A) = a(ei - fh) - b(di - fg) + c(dh - eg)
$  

**Exemple :**  
$
A = \begin{pmatrix}
1 & 2 & 3 \\
0 & 1 & 4 \\
5 & 6 & 0
\end{pmatrix}
$  
$ \det(A) = 1(1 \cdot 0 - 4 \cdot 6) - 2(0 \cdot 0 - 4 \cdot 5) + 3(0 \cdot 6 - 1 \cdot 5) = -24 + 40 - 15 = 1 \quad (\text{inversible}) $  

###### **Étape 2 : Matrice des Cofacteurs**  
Le **cofacteur** \( C_{ij} \) de \( a_{ij} \) est :  
$
C_{ij} = (-1)^{i+j} \det(M_{ij})
$  
où $ M_{ij} $ est la sous-matrice obtenue en supprimant la ligne $ i $ et la colonne $ j $.  

**Exemple (pour \( A \) ci-dessus) :**  
$ C_{11} = (-1)^{1+1} \det \begin{pmatrix} 1 & 4 \\ 6 & 0 \end{pmatrix} = -24  $  
$ C_{12} = (-1)^{1+2} \det \begin{pmatrix} 0 & 4 \\ 5 & 0 \end{pmatrix} = 20  $  
$ C_{13} = (-1)^{1+3} \det \begin{pmatrix} 0 & 1 \\ 5 & 6 \end{pmatrix} = -5  $  
On construit ainsi la **matrice des cofacteurs** $ C $.  

#### **Étape 3 : Matrice Adjointe**  
La **matrice adjointe** $ \text{adj}(A) $ est la transposée de la matrice des cofacteurs :  
$
\text{adj}(A) = C^T
$

#### **Étape 4 : Calcul de l’Inverse**  
$
A^{-1} = \frac{1}{\det(A)} \text{adj}(A)
$ 

**Exemple complet :**  
$
A^{-1} = \frac{1}{1} \begin{pmatrix}
-24 & 20 & -5 \\
20 & -15 & 4 \\
-5 & 4 & -1
\end{pmatrix}^T = \begin{pmatrix}
-24 & 20 & -5 \\
20 & -15 & 4 \\
-5 & 4 & -1
\end{pmatrix}
$ 

































# **1.2.3. Calcul de l’Inverse d’une Matrice**  

## **1. Introduction : Pourquoi Calculer l’Inverse d’une Matrice ?**  
L’inverse d’une matrice est un concept fondamental en algèbre linéaire, particulièrement utile en **intelligence artificielle** pour :  
- **Résoudre des systèmes d’équations linéaires** (alternative à la méthode du pivot de Gauss).  
- **Trouver des solutions optimales** dans les problèmes d’optimisation (régression linéaire, moindres carrés).  
- **Effectuer des transformations géométriques inverses** (en vision par ordinateur, robotique).  

**Définition :**  
Une matrice carrée \( A \) d’ordre \( n \) est **inversible** s’il existe une matrice \( B \) telle que :  
\[
A \times B = B \times A = I_n
\]  
où \( I_n \) est la matrice identité. On note \( B = A^{-1} \).  

---

## **2. Conditions d’Inversibilité**  
Une matrice \( A \) est inversible **si et seulement si** :  
- Son **déterminant est non nul** (\( \det(A) \neq 0 \)).  
- Ses colonnes (ou lignes) sont **linéairement indépendantes**.  

---

## **3. Méthodes pour Calculer l’Inverse d’une Matrice**  

### **3.1. Méthode du Déterminant et des Cofacteurs (Matrice Adjointe)**  
#### **Étape 1 : Calcul du Déterminant**  
Le déterminant d’une matrice \( 3 \times 3 \) :  
\[
A = \begin{pmatrix}
a & b & c \\
d & e & f \\
g & h & i
\end{pmatrix}
\]  
est donné par :  
\[
\det(A) = a(ei - fh) - b(di - fg) + c(dh - eg)
\]  

**Exemple :**  
\[
A = \begin{pmatrix}
1 & 2 & 3 \\
0 & 1 & 4 \\
5 & 6 & 0
\end{pmatrix}
\]  
\[
\det(A) = 1(1 \cdot 0 - 4 \cdot 6) - 2(0 \cdot 0 - 4 \cdot 5) + 3(0 \cdot 6 - 1 \cdot 5) = -24 + 40 - 15 = 1 \quad (\text{inversible})
\]  

#### **Étape 2 : Matrice des Cofacteurs**  
Le **cofacteur** \( C_{ij} \) de \( a_{ij} \) est :  
\[
C_{ij} = (-1)^{i+j} \det(M_{ij})
\]  
où \( M_{ij} \) est la sous-matrice obtenue en supprimant la ligne \( i \) et la colonne \( j \).  

**Exemple (pour \( A \) ci-dessus) :**  
\[
C_{11} = (-1)^{1+1} \det \begin{pmatrix} 1 & 4 \\ 6 & 0 \end{pmatrix} = -24  
\]  
\[
C_{12} = (-1)^{1+2} \det \begin{pmatrix} 0 & 4 \\ 5 & 0 \end{pmatrix} = 20  
\]  
\[
C_{13} = (-1)^{1+3} \det \begin{pmatrix} 0 & 1 \\ 5 & 6 \end{pmatrix} = -5  
\]  
On construit ainsi la **matrice des cofacteurs** \( C \).  

#### **Étape 3 : Matrice Adjointe**  
La **matrice adjointe** \( \text{adj}(A) \) est la transposée de la matrice des cofacteurs :  
\[
\text{adj}(A) = C^T
\]  

#### **Étape 4 : Calcul de l’Inverse**  
\[
A^{-1} = \frac{1}{\det(A)} \text{adj}(A)
\]  

**Exemple complet :**  
\[
A^{-1} = \frac{1}{1} \begin{pmatrix}
-24 & 20 & -5 \\
20 & -15 & 4 \\
-5 & 4 & -1
\end{pmatrix}^T = \begin{pmatrix}
-24 & 20 & -5 \\
20 & -15 & 4 \\
-5 & 4 & -1
\end{pmatrix}
\]  

### **3.2. Méthode par Pivot de Gauss (Élimination Linéaire)**  
On transforme \( [A | I_n] \) en \( [I_n | A^{-1}] \) via des opérations élémentaires.  

**Exemple :**  
\[
\begin{pmatrix}
1 & 2 & 3 & | & 1 & 0 & 0 \\
0 & 1 & 4 & | & 0 & 1 & 0 \\
5 & 6 & 0 & | & 0 & 0 & 1
\end{pmatrix} \xrightarrow{\text{opérations}} \begin{pmatrix}
1 & 0 & 0 & | & -24 & 20 & -5 \\
0 & 1 & 0 & | & 20 & -15 & 4 \\
0 & 0 & 1 & | & -5 & 4 & -1
\end{pmatrix}
\]  

---

## **4. Applications de l’Inverse Matriciel**  

### **4.1. Résolution de Systèmes Linéaires**  
Si \( AX = B \), alors \( X = A^{-1}B \).  

**Exemple :**  
\[
\begin{cases}
x + 2y + 3z = 6 \\
y + 4z = 9 \\
5x + 6y = 3
\end{cases} \Rightarrow X = A^{-1} \begin{pmatrix} 6 \\ 9 \\ 3 \end{pmatrix} = \begin{pmatrix} -24 \cdot 6 + 20 \cdot 9 -5 \cdot 3 \\ 20 \cdot 6 -15 \cdot 9 +4 \cdot 3 \\ -5 \cdot 6 +4 \cdot 9 -1 \cdot 3 \end{pmatrix} = \begin{pmatrix} 1 \\ -3 \\ 3 \end{pmatrix}
\]  

### **4.2. Calcul de Transformations Inverses**  
En **graphique 3D**, si une matrice \( M \) représente une rotation, \( M^{-1} \) permet de revenir à la position initiale.  

### **4.3. Régression Linéaire (Moindres Carrés)**  
La solution de \( \theta = (X^T X)^{-1} X^T Y \) nécessite l’inversion de \( X^T X \).  

---

## **Exercices d’Application**  

### **Exercice 1 (Inverse par Cofacteurs)**  
Calculer \( A^{-1} \) pour :  
\[
A = \begin{pmatrix}
2 & 0 & -1 \\
3 & 1 & 4 \\
-2 & 5 & 7
\end{pmatrix}
\]  

### **Exercice 2 (Résolution de Système)**  
Résoudre en utilisant \( A^{-1} \) :  
\[
\begin{cases}
2x - z = 1 \\
3x + y + 4z = 0 \\
-2x + 5y + 7z = 3
\end{cases}
\]  

### **Exercice 3 (Inverse par Gauss)**  
Trouver \( B^{-1} \) par pivot de Gauss :  
\[
B = \begin{pmatrix}
1 & 0 & 2 \\
-1 & 3 & 1 \\
2 & 4 & 0
\end{pmatrix}
\]  

---

## **Corrigés (Extraits)**  

### **Corrigé Exercice 1**  
\[
\det(A) = 2(1 \cdot 7 - 4 \cdot 5) - 0 + (-1)(3 \cdot 5 - 1 \cdot (-2)) = -46  
\]  
\[
A^{-1} = -\frac{1}{46} \begin{pmatrix}
-13 & 5 & 1 \\
29 & 12 & -11 \\
17 & -10 & 2
\end{pmatrix}
\]  

### **Corrigé Exercice 2**  
\[
X = A^{-1} \begin{pmatrix} 1 \\ 0 \\ 3 \end{pmatrix} = \begin{pmatrix} 1 \\ -2 \\ 1 \end{pmatrix}
\]  

### **Corrigé Exercice 3**  
\[
B^{-1} = \begin{pmatrix}
-4 & 8 & -6 \\
2 & -4 & 3 \\
10 & -4 & 3
\end{pmatrix}
\]  

---

**Conclusion :** L’inversion matricielle est cruciale en IA pour la résolution de problèmes linéaires et l’optimisation. Les méthodes présentées (déterminant, cofacteurs, Gauss) sont des outils de base en algèbre linéaire appliquée.
