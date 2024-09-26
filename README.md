from datetime import datetime

class Cliente:
    def __init__(self, nome, cpf):
        self.nome = nome
        self.cpf = cpf
        self.conta = Conta()

class Conta:
    def __init__(self):
        self.saldo = 0
        self.extrato = []
        self.numero_saques = 0
        self.LIMITE_SAQUES = 3
        self.limite = 100
        self.limite_diario = 500
        self.total_sacado_hoje = 0

    def depositar(self, valor):
        if valor > 0:
            self.saldo += valor
            self.extrato.append(f"{datetime.now()} - Depósito: R$ {valor:.2f}")
            print(f"Depósito de R$ {valor:.2f} realizado com sucesso!")
        else:
            print("Operação falhou! O valor informado é inválido.")

    def sacar(self, valor):
        excedeu_saldo = valor > self.saldo
        excedeu_limite = valor > self.limite
        excedeu_saques = self.numero_saques >= self.LIMITE_SAQUES
        excedeu_limite_diario = self.total_sacado_hoje + valor > self.limite_diario

        if excedeu_saldo:
            print("Operação falhou! Você não tem saldo suficiente.")
        elif excedeu_limite:
            print("Operação falhou! O valor do saque excede o limite.")
        elif excedeu_saques:
            print("Operação falhou! Número máximo de saques chegou ao limite.")
        elif excedeu_limite_diario:
            print("Operação falhou! Limite diário de saques excedido.")
        elif valor > 0:
            self.saldo -= valor
            self.extrato.append(f"{datetime.now()} - Saque: R$ {valor:.2f}")
            self.numero_saques += 1
            self.total_sacado_hoje += valor
            print(f"Saque de R$ {valor:.2f} realizado com sucesso!")
        else:
            print("Operação falhou! O valor informado é inválido.")

    def mostrar_extrato(self):
        print("\n================ EXTRATO ================")
        if not self.extrato:
            print("Não foram realizadas transações.")
        else:
            for transacao in self.extrato:
                print(transacao)
        print(f"\nSaldo: R$ {self.saldo:.2f}")
        print("==========================================")

def mostrar_menu():
    menu = """
    [c] Cadastrar Cliente
    [d] Depositar
    [s] Sacar
    [e] Extrato
    [q] Sair
    => """
    return input(menu)

def main():
    clientes = {}

    while True:
        opcao = mostrar_menu()

        if opcao == "c":
            nome = input("Informe o nome do cliente: ")
            cpf = input("Informe o CPF do cliente: ")
            if cpf not in clientes:
                clientes[cpf] = Cliente(nome, cpf)
                print(f"Cliente {nome} cadastrado com sucesso!")
            else:
                print("Cliente já cadastrado.")

        elif opcao == "d":
            cpf = input("Informe o CPF do cliente: ")
            if cpf in clientes:
                valor = float(input("Informe o valor do depósito: "))
                clientes[cpf].conta.depositar(valor)
            else:
                print("Cliente não encontrado.")

        elif opcao == "s":
            cpf = input("Informe o CPF do cliente: ")
            if cpf in clientes:
                valor = float(input("Informe o valor do saque: "))
                clientes[cpf].conta.sacar(valor)
            else:
                print("Cliente não encontrado.")

        elif opcao == "e":
            cpf = input("Informe o CPF do cliente: ")
            if cpf in clientes:
                clientes[cpf].conta.mostrar_extrato()
            else:
                print("Cliente não encontrado.")

        elif opcao == "q":
            break
        else:
            print("Operação inválida, por favor selecione a opção novamente.")

if __name__ == "__main__":
    main()

