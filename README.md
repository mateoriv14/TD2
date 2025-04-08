
class Polynomial:
    def __init__(self,n):
       self.n=n
    
    def __str__(self):
        """Retourne une représentation sous forme de chaîne du polynôme."""
        terms = []
        i = 0
        while i < len(self.n):
            coef = self.n[i]
            if coef != 0:
                term = f"{coef}"
                if i > 0:
                    term += f"x^{i}" if i > 1 else "x"
                terms.append(term)
            i += 1
        return " + ".join(terms) if terms else "0"
    
    def __add__(self, other):
       """Additionne deux polynômes."""
       max_length = max(len(self.n), len(other.n))
       new_coeffs = [
           (self.n[i] if i < len(self.n) else 0) +
           (other.n[i] if i < len(other.n) else 0)
           for i in range(max_length)
       ]
       
       
       
#%%

class Poly:
    def __init__(self, coeffs, q, n):
        self.q = q
        self.n = n
        self.coeffs = [c % q for c in coeffs[:n]]
        while len(self.coeffs) < n:
            self.coeffs.append(0)

    def __add__(self, other):
        return Poly([(a + b) % self.q for a, b in zip(self.coeffs, other.coeffs)], self.q, self.n)

    def __mul__(self, other):
        tmp = [0] * (2 * self.n)
        for i in range(self.n):
            for j in range(self.n):
                tmp[i + j] = (tmp[i + j] + self.coeffs[i] * other.coeffs[j]) % self.q
        # Réduction modulo X^n + 1
        for i in range(self.n, 2 * self.n):
            tmp[i - self.n] = (tmp[i - self.n] - tmp[i]) % self.q
        return Poly(tmp[:self.n], self.q, self.n)
    def scalar(self, c):
       """Multiplie tous les coefficients par un scalaire c modulo q"""
       return Poly([(c * a) % self.q for a in self.coeffs], self.q, self.n)

    def rescale(self, r):
       """Change l’anneau de Z_q à Z_r tout en gardant les mêmes coefficients"""
       return Poly([c % r for c in self.coeffs], r, self.n)


    def __repr__(self):
        return " + ".join(f"{c}X^{i}" for i, c in enumerate(self.coeffs) if c) or "0"
# Exemple
if __name__ == "__main__":
    p = Poly([1, 2, 3], 7, 4)
    print("p =", p)
    print("3 * p =", p.scalar(3))
    print("rescale p to Z_5 =", p.rescale(5))
