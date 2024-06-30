# Cadastro-de-pacientes
import PySimpleGUI as sg
import pandas as pd
from datetime import datetime


def salvar_dados(nome_arquivo, dados):
    df = pd.DataFrame(dados)
    df.to_excel(nome_arquivo, index=False)


def carregar_dados(nome_arquivo):
    try:
        df = pd.read_excel(nome_arquivo)
        return df.values.tolist()
    except FileNotFoundError:
        return []


def main():
    sg.theme('LightGreen3')  # Escolha do tema para a interface

    layout = [
        [sg.Text('Cadastro de Pacientes')],
        [sg.Text('Nome'), sg.InputText(key='-NOME-')],
        [sg.Text('CEP'), sg.InputText(key='-CEP-')],
        [sg.Text('Bairro'), sg.InputText(key='-BAIRRO-')],
        [sg.Text('Gênero'), sg.InputText(key='-GENERO-')],
        [sg.Text('Casa'), sg.InputText(key='-CASA-')],
        [sg.Text('Documento'), sg.InputText(key='-DOCUMENTO-')],
        [sg.Text('Data de Nascimento'), sg.InputText(key='-NASCIMENTO-')],
        [sg.Text('Telefone'), sg.InputText(key='-TELEFONE-')],
        [sg.Button('Registrar Entrada'), sg.Button('Salvar'), sg.Button('Carregar')],
        [sg.Text('Registros:', size=(40, 1))],
        [sg.Output(size=(60, 10))]
    ]

    window = sg.Window('Sistema de Recepcão Hospitalar', layout)

    dados = []

    while True:
        event, values = window.read()

        if event == sg.WINDOW_CLOSED:
            break

        if event == 'Registrar Entrada':
            registro = {
                'Nome': values['-NOME-'],
                'CEP': values['-CEP-'],
                'Bairro': values['-BAIRRO-'],
                'Gênero': values['-GENERO-'],
                'Casa': values['-CASA-'],
                'Documento': values['-DOCUMENTO-'],
                'Nascimento': values['-NASCIMENTO-'],
                'Telefone': values['-TELEFONE-'],
                'Entrada': datetime.now().strftime('%Y-%m-%d %H:%M:%S')
            }
            dados.append(registro)
            print(f"Entrada registrada para {values['-NOME-']} às {datetime.now().strftime('%H:%M:%S')}")

        if event == 'Salvar':
            nome_arquivo = sg.popup_get_file('Salvar como:', save_as=True, file_types=(("Arquivos XLSX", "*.xlsx"),))
            if nome_arquivo:
                salvar_dados(nome_arquivo, dados)
                print(f"Dados salvos em {nome_arquivo}")

        if event == 'Carregar':
            nome_arquivo = sg.popup_get_file('Selecionar arquivo:', file_types=(("Arquivos XLSX", "*.xlsx"),))
            if nome_arquivo:
                dados_carregados = carregar_dados(nome_arquivo)
                print(f"Dados carregados de {nome_arquivo}:")
                for dado in dados_carregados:
                    print(dado)

    window.close()


if __name__ == '__main__':
    main()
