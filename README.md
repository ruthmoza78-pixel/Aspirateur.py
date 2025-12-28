# Aspirateur.py
import tkinter as tk
import time

class Aspirateur:
    def __init__(self, canvas):
        self.canvas = canvas
        self.x = 50  # Position initiale sur l'axe X
        self.y = 50  # Position initiale sur l'axe Y
        self.size = 10  # Taille de l'aspirateur
        self.chambreA_sale = True  # État initial de la chambre A
        self.chambreB_sale = True  # État initial de la chambre B
        self.current_chambre = 'A'  # Chambre actuelle
        self.update()

    def update(self):
        self.canvas.delete("all")  # Efface le canvas

        # Dessine les chambres
        self.dessiner_chambre(20, 20, 'A')
        self.dessiner_chambre(200, 20, 'B')

        # Dessine l'aspirateur
        if self.chambreA_sale or self.chambreB_sale:
            self.canvas.create_oval(self.x, self.y, self.x + self.size, self.y + self.size, fill='orange')

        self.after = self.canvas.after(2000, self.deplacer)

    def dessiner_chambre(self, x, y, chambre):
        color = 'red' if (self.chambreA_sale if chambre == 'A' else self.chambreB_sale) else 'green'
        self.canvas.create_rectangle(x, y, x + 150, y + 100, fill=color)
        if color == 'red':
            self.canvas.create_text(x + 75, y + 50, text=f"Chambre {chambre} (sale)", fill="white")
        else:
            self.canvas.create_text(x + 75, y + 50, text=f"Chambre {chambre} (propre)", fill="black")

    def deplacer(self):
        # Logique de nettoyage
        if self.chambreA_sale:
            self.current_chambre = 'A'
            self.x = 70  # Position pour la chambre A
            self.chambreA_sale = False  # Nettoie la chambre A
        elif self.chambreB_sale:
            self.current_chambre = 'B'
            self.x = 220  # Position pour la chambre B
            self.chambreB_sale = False  # Nettoie la chambre B
        
        self.update()


if __name__ == "__main__":
    root = tk.Tk()
    root.title("Agent Aspirateur Intelligent")

    canvas = tk.Canvas(root, width=400, height=200, bg='white')
    canvas.pack()

    aspirateur = Aspirateur(canvas)

    root.mainloop()
