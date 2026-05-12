from abc import ABC, abstractmethod
import random

class Dinosaurio(ABC):
    def __init__(self, nombre, tipo):
        self.nombre = nombre
        self.__tipo = tipo


        self.vida = random.randint(80, 100)
        self.ataque = random.randint(10, 30)
        self.defensa = random.randint(5, 20)
        self.velocidad = random.randint(1, 2)

        self.habilidades = []

    
    def agregar_habilidad(self, habilidad):
        self.habilidades.append(habilidad)

    def mostrar_habilidades(self):
        for i, h in enumerate(self.habilidades):
            print(f"{i}: {h.nombre}")
    
    def usar_habilidad(self, index, objetivo=None):
        if 0 <= index < len(self.habilidades):
            self.habilidades[index].usar(self, objetivo)
        else:
            print("Habilidad inválida")


    def atributos(self):
        print(f"Nombre: {self.nombre}")
        print(f"vida: {self.vida}")
        print (f"ataque: {self.ataque}")
        print (f"defensa: {self.defensa}")
        print (f"velocidad: {self.velocidad}")
        print (f"Tipo: {self.__tipo}")
        print()

    @abstractmethod
    def prioridad(self):
        pass

    def daño(self, enemigo):
        return max(0, self.ataque - enemigo.defensa)
    
    def estaVivo (self ):
        return self.vida > 0

    def muerto (self):
        print(f"Ha muerto {self.nombre}")

    def atacar (self, enemigo):

        if not self.estaVivo():
            print (f"{self.nombre} está muerto y no puede atacar")
            return
        
        if not enemigo.estaVivo():
            enemigo.muerto()
            return  
        
        if enemigo.esquivar():
            print(f"{enemigo.nombre} esquivó el ataque")
            return
        
        # usamos el polimorfismo aqui porque llamamos a los mismos metodos pero
        # dependiendo del objeto se ejecuta de manera diferente 
        daño = self.daño(enemigo)
        daño = self.critico(daño)
        daño = enemigo.aplicar_defensa(daño)
        enemigo.vida = max(0, enemigo.vida - daño)

        print(f"{self.nombre} ataco a {enemigo.nombre} y le causo {daño} de daño")
        print(f"la vida de {enemigo.nombre} es {enemigo.vida}")
        if not enemigo.estaVivo():
            enemigo.muerto()
        return max(0, enemigo.vida)

    def aplicar_defensa (self, daño):
        if random.random() < 0.1: #10% de BLOQUEAR 
             print(f"{self.nombre} Bloqueo el ataque")
             return 0
        return daño
    
    def esquivar(self):
        prob = 0.05 + (self.velocidad / 200)#5% de probabilidad de ESQUIVAR
        return random.random() < prob 

    def critico(self, daño):
        if random.random() < 0.1: #10% de probabilidad de un CRITICO
            print(f"{self.nombre} HIZO UN CRITICO")
            return daño * 2
        return daño



# ---------------------------------------- Clases hijas --------------------------------------

class Jugador(Dinosaurio):
    def __init__(self, nombre):

        super().__init__(nombre, "Jugador")

        self.punto = 100

        self.agregar_habilidad(AtaqueEspecial("Ataque de lanza", 25))
        self.agregar_habilidad(Curacion("Curacion de hierbas", 20))

    def atributos(self):
        
        super().atributos()

        print(f"Puntos: {self.punto}")
        print("Tienes habilidades especiales que puedes usar en batalla")

    def prioridad(self):
        return self.velocidad

    
    def ganar_puntos(self):
        self.punto += 20
        print(f"{self.nombre} gano 20 puntos")
    
    def perder_puntos(self):

        self.punto -= 15
        print(f"{self.nombre} perdio 15 puntos")

    def recuperar_vida(self):


        vida_anterior = self.vida

        self.vida += 50

        if self.vida > 100:
            self.vida = 100

        recuperada = self.vida - vida_anterior

        print(f"{self.nombre} recuperó {recuperada} de vida")



class Carnivoro(Dinosaurio):
    def __init__(self):
        super().__init__("REX", "Carnivoro")

        self.agregar_habilidad(AtaqueEspecial("Mordisco Mortal", 20))
        self.bonus_daño = 10

    def prioridad(self):
        return self.velocidad 
        
    def atributos(self):
        super().atributos()
        print(f"Conseguiste Bonus de daño +{self.bonus_daño}")
        print("Ademas de alta probabilidad de critico")


    def daño(self,enemigo):
        daño_base = super().daño(enemigo)
        return daño_base + self.bonus_daño
    
    def critico(self, daño):
        if random.random() < 0.3: # 30% de probabilidad de critico
            print(f"{self.nombre} HIZO UN MEGA CRITICO")
            return daño * 2
        return daño


class Herbivoro(Dinosaurio):
    def __init__(self):
        super().__init__("Triceratops", "Herbivoro")
        self.agregar_habilidad(Curacion("Recuperar", 30))
        self.bonus_defensa = 20

    def prioridad(self):
        return self.velocidad 
   
    
    def atributos (self):
        super().atributos()
        print(f"Conseguiste Bonus de defensa +{self.bonus_defensa}")
        print("Se aumento la probabilidad de bloquear un ataque")
    
    def aplicar_defensa (self, daño):
        if random.random() < 0.4:
            print(f"{self.nombre} BLOQUEO EL ATAQUE")
            return 0
        return max(0, daño - self.bonus_defensa)
    

class Volador(Dinosaurio):
    def __init__(self):
        super().__init__("Pterodactilo", "Volador")

        self.bonus_velocidad = 10


    def prioridad(self):
        return self.velocidad == random.randint(1, 3) # los voladores son mas rapidos que los demas

    def atributos(self):
        super().atributos()
        print(f"Conseguiste un bonus de velocidad +{self.bonus_velocidad}")
        print("Se Aumenta la probabilidad de esquivar ataques")

    def esquivar(self):
        prob = 0.2 + (self.velocidad / 100)
        return random.random() < prob

class Acuatico(Dinosaurio):
    def __init__(self):
        super().__init__("Mosasaurio", "Acuatico")

    
    def prioridad(self):
        return self.velocidad
#veneno

# GOMER ARREGLA LO DE ESQUIVAR, BLOQUEAR Y CRITICO QUITALO O NO SE PERO ESO HAY QUE ARREGLARLO


class Mitico(Dinosaurio):
    def __init__(self):
        super().__init__("Dragon", "Mitico")
    
    def prioridad(self):
        return self.velocidad
        #fuego 


# ------------------------ HABILIDADES Y ATAQUES ESPECIALES -------------------------
class Habilidad:
    def __init__(self, nombre):
        self.nombre = nombre
    
    def usar (self, usuario, objetivo):
        pass

class AtaqueEspecial(Habilidad):
    def __init__(self, nombre, daño_extra):
        super().__init__(nombre)
        self.daño_extra = daño_extra
    
    def usar(self, usuario, objetivo):
        daño = usuario.daño(objetivo)
        daño += self.daño_extra
        daño = usuario.critico(daño)
        daño = objetivo.aplicar_defensa(daño)
        objetivo.vida = max(0, objetivo.vida - daño)
        print(f"{usuario.nombre} uso {self.nombre} e hizo {daño} de daño")

class Curacion(Habilidad):
    def __init__(self, nombre, curacion):
        super().__init__(nombre)
        self.curacion = curacion
    
    def usar(self, usuario, objetivo=None):
        usuario.vida = min(100, usuario.vida + self.curacion)
        print(f"{usuario.nombre} se curo {self.curacion} de vida")


# ----------------------------- BATALLA -------------------------------
class Batalla:

    def __init__(self, dino1, dino2):
        self.dino1 = dino1
        self.dino2 = dino2
        self.turno = 1

    def decidir_orden(self):

        if self.dino1.prioridad() > self.dino2.prioridad():
            return self.dino1, self.dino2

        else:
            return self.dino2, self.dino1

    def turno_dinosaurio(self, atacante, defensor):

        print(f"\nTurno de {atacante.nombre}")
        
        #si el que juega es el player
        if isinstance(atacante, Jugador):

            print("1. Atacar")
            print("2. Usar habilidad")
            
            opcion = input("Seleccione acción: ")
            
            if opcion == "1":

                atacante.atacar(defensor)

            elif opcion == "2":

                if len(atacante.habilidades) == 0:

                    print("No tienes habilidades, ataca automaticamente")
                    atacante.atacar(defensor)

                else:
                    atacante.mostrar_habilidades()

                    try:

                        index = int(input("elige una habilidad: "))
                        atacante.usar_habilidad(index, defensor)
                    
                    except:

                        print("opcion invalida")
                        atacante.atacar(defensor)

            else:
                print("Opcion invalida")
                atacante.atacar(defensor)
        
        #Si es un bot
        else:

            accion = random.randint(1,2)

            if accion == 1:

                print(f"{atacante.nombre} decidio atacar")
                atacante.atacar(defensor)
            
            else:

                if len(atacante.habilidades) > 0: 

                    habilidad_random = random.randint(0, len(atacante.habilidades) - 1)

                    print(f"{atacante.nombre} usó una habilidad")
                    atacante.usar_habilidad(habilidad_random, defensor)

                else:

                    atacante.atacar(defensor)


    def iniciar(self):
        
        print("\n===== INICIO DE LA BATALLA =====")

        primero, segundo = self.decidir_orden()

        while self.dino1.estaVivo() and self.dino2.estaVivo():

            print(f"\n--- Turno {self.turno} ---")

            # Turno del primero
            self.turno_dinosaurio(primero, segundo)

            if not segundo.estaVivo():
                break

            # Turno del segundo
            self.turno_dinosaurio(segundo, primero)

            if not primero.estaVivo():
                break

            self.turno += 1

        self.mostrar_ganador()

    def mostrar_ganador(self):

        print("\n===== FIN DE LA BATALLA =====")

        if self.dino1.estaVivo():

            print(f"Ganador: {self.dino1.nombre}")

        else:

            print(f"Ganador: {self.dino2.nombre}")

    
#-------------------------------- MAIN ----------------------------------

nombre = input("Ingrese el nombre de su jugador: ")

player = Jugador(nombre)

print( "\n================= TU PERSONAJE =================")
player.atributos()

enemigos = [Carnivoro, Herbivoro, Volador, Acuatico, Mitico]

for pelea in range(1, 6):

    print(f"\n===== BATALLA {pelea} =====")

    enemigo = random.choice(enemigos)()

    print(f"\nEnemigo encontrado: ")
    enemigo.atributos()

    batalla = Batalla(player, enemigo)
    
    batalla.iniciar()

    if player.estaVivo():
        player.ganar_puntos()
    
    else:

        player.perder_puntos()
        player.recuperar_vida()
    
print(f"\n=============fin del juego==============")
print(f"puntajes finales {player.punto}")
