# Atividade-POO-2
from abc import ABC, abstractmethod

# --- 1. INTERFACE ---
class ConectavelIoT(ABC):
    @abstractmethod
    def enviar_dados_rede(self):
        pass

# --- 2. CLASSE ABSTRATA ---
class Sensor(ABC):
    def __init__(self, nome, valor_inicial):
        self.nome = nome
        self._valor = valor_inicial 

    @abstractmethod
    def ler_dados(self, novo_valor):
        pass

# --- 3. CLASSES FILHAS ---

class SensorTemperatura(Sensor, ConectavelIoT):
    def __init__(self, valor_inicial):
        super().__init__("Temperatura", valor_inicial)

    def ler_dados(self, novo_valor):
        if novo_valor >= -273.15:
            self._valor = novo_valor

    def enviar_dados_rede(self):
        print(f"[REDE] {self.nome}: {self._valor}°C")

class SensorUmidade(Sensor, ConectavelIoT):
    def __init__(self, valor_inicial):
        super().__init__("Umidade", valor_inicial)

    def ler_dados(self, novo_valor):
        if 0 <= novo_valor <= 100:
            self._valor = novo_valor

    def enviar_dados_rede(self):
        print(f"[REDE] {self.nome}: {self._valor}%")

class SensorPH(Sensor, ConectavelIoT):
    def __init__(self, valor_inicial):
        super().__init__("pH", valor_inicial)

    def ler_dados(self, novo_valor):
        if 0 <= novo_valor <= 14:
            self._valor = novo_valor

    def enviar_dados_rede(self):
        print(f"[REDE] {self.nome}: {self._valor}")

# --- 4. CENTRAL DE CONTROLE ---
class CentralDeControle:
    def __init__(self):
        self.sensores = []

    def adicionar_sensor(self, sensor):
        self.sensores.append(sensor)

    def processar_todos(self):
        print("\n--- TRANSMISSÃO IOT ---")
        for s in self.sensores:
            if isinstance(s, ConectavelIoT):
                s.enviar_dados_rede()
        print("-----------------------\n")

# --- 5. EXECUÇÃO ---
if __name__ == "__main__":
    central = CentralDeControle()

    # Criando os sensores com valores iniciais
    s1 = SensorTemperatura(25.0)
    s2 = SensorUmidade(50.0)
    s3 = SensorPH(7.0)

    # Testando as regras (Invariantes)
    s1.ler_dados(38.5)   # Válido
    s2.ler_dados(120.0)  # Inválido (Liskov: mantém 50.0)
    s3.ler_dados(14.5)   # Inválido (Liskov: mantém 7.0)

    central.adicionar_sensor(s1)
    central.adicionar_sensor(s2)
    central.adicionar_sensor(s3)

    central.processar_todos()

# PROVA DE LISKOV: As subclasses mantêm a integridade dos dados (invariantes) 
# definidas para cada tipo de sensor, sem quebrar o comportamento esperado pela Central.
