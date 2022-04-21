#Daniel Passamani e Luis Santos

class MaxHeap:
    def __init__(self):
        self.heap = [0]

    def input_pacient(self, item):
        self.heap.append(item)
        self.__floatUp(len(self.heap) - 1)

    def get_pacient(self):
        if len(self.heap) > 2:
            self.__swap(1, len(self.heap) - 1)
            max = self.heap.pop()
            self.__bubbleDown(1)
        elif len(self.heap) == 2:
            max = self.heap.pop()
        else:
            max = False
        return max

    def peek(self):
        if self.heap[1]:
            return self.heap[1]
        return False

    def __swap(self, i, j):
        self.heap[i], self.heap[j] = self.heap[j], self.heap[i]

    def __floatUp(self, index):
        parent = index//2
        if index <= 1:
            return
        elif self.heap[index] > self.heap[parent]:
            self.__swap(index, parent)
            self.__floatUp(parent)

    def __bubbleDown(self, index):
        left = index * 2
        right = index * 2 + 1
        bigger = index
        if len(self.heap) > left and self.heap[bigger] < self.heap[left]:
            bigger = left
        if len(self.heap) > right and self.heap[bigger] < self.heap[right]:
            bigger = right

        if bigger != index:
            self.__swap(index, bigger)
            self.__bubbleDown(bigger)

class Pacient:
    def __init__(self, name, typeBlood, dateNasc):
        self.pacient = (name, typeBlood, dateNasc)
    
    def returnPacient(self):
        return self.pacient

h = MaxHeap()
x = True
token = 999
listAttend = []
while x:
    resp = int(input('\n1 - Adicionar novo paciente;\n2 - Chamar próximo paciente;\n3 - Mostrar próximo paciente;\n4 - Listar os 5 últimos chamados\n'))
    if (resp == 1):
        full_name = input('\nDigite o nome: ')
        blood_type = input('\nDigite o tipo sanguíneo: ')
        birth_date = input('\nDigite a data de nascimento: ')
        pacient = Pacient(full_name, blood_type, birth_date).returnPacient()
        (name, blood, dateNasc) = pacient
        urgency = int(input('\nDigite a urgência do caso (1 a 10): '))
        h.input_pacient((urgency, token, name, blood, dateNasc))
        token -= 1
    elif (resp == 2):
        try:
            pacient = h.get_pacient()
            print('Nome: '+pacient[2]+'\nSangue: '+pacient[3]+'\nData de Nascimento: '+pacient[4]+'\nUrgência: '+str(pacient[0])+'\nFicha: '+str(pacient[1])+'\n\n')
            listAttend.append(pacient)
        except:
            print('Sem pacientes a ser chamados')
    elif (resp == 3):
        try:
            pacient = h.peek()
            print('Nome: '+pacient[2]+'\nSangue: '+pacient[3]+'\nData de Nascimento: '+pacient[4]+'\nUrgência: '+str(pacient[0])+'\nFicha: '+str(pacient[1])+'\n\n')
        except:
            print('Sem pacientes a ser mostrados')
    elif (resp == 4):
        print('\n--- Pacientes Atendidos ({}) ---'.format(len(listAttend)))
        if (len(listAttend)>5):
            for item in range(len(listAttend)-1,len(listAttend)-5,-1):
                print(item)
                pacient = listAttend[item]
                print('Nome: '+pacient[2]+'\nSangue: '+pacient[3]+'\nData de Nascimento: '+pacient[4]+'\nUrgência: '+str(pacient[0])+'\nFicha: '+str(pacient[1])+'\n\n')
        else:
            for pacient in listAttend:
                print('Nome: '+pacient[2]+'\nSangue: '+pacient[3]+'\nData de Nascimento: '+pacient[4]+'\nUrgência: '+str(pacient[0])+'\nFicha: '+str(pacient[1])+'\n\n')
    else:
        print()
