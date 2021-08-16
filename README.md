# cep

import pandas as pd
import googlemaps as gm
import requests

dados = pd.read_csv('ARQUIVO.csv', sep =';')

dados['Endereco1'] = dados['Endereco'] + ', '+ dados['Numero'] + ', ' + dados['Bairro'] + ', ' + dados['Descricao']

dados.Endereco1.fillna('erro', inplace = True)

API_KEY = 'API_GOOGLE'
address1 = list(dados['Endereco1'])

k = []
j = []
for i in address1:
    params = dict(
        address = i

    )
    
    for x in i:
    
        i = params
    #for z in range(len(i)):
            
        #print(z)
    for y in params: 
    

        base_url = 'https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyARuS0UXjtWXThUyKL9KWfrFqVT7TDsmSI&'
        response = requests.get(base_url, params=params)
        
        

        #data = response.json()['results'][0]['formatted_address']
        #data2 = response.json()['results'][0]['address_components'][-1]['long_name'] 
        #k.append(data)
        #j.append(data2)
        data = response.json()
        if data['status'] == 'OK':
            data1 = data['results'][0]['address_components'][-1]['long_name']
            data2 = data['results'][0]['formatted_address']
        else:  
            k.append('erro')
            j.append('erro')
        #print(data2)
        k.append(data1)
        j.append(data2)
        
dfk = pd.DataFrame(k)
dfj = pd.Series(j)
dfk['Endereco'] = dfj
dfk.rename(columns = {0:'CEP'},inplace = True)

dfk.to_excel('dadoscepFelix.xlsx')
