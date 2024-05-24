# Proyecto-Final
Impacto ambiental de las emisiones de CO2 en las salas de operación hospitalarias

 function GraficarPushed(app)
            co2_total_vec=zeros(1,100);
            
            for i = height(co2_total_vec)
                enfermedad = app.EnfermedadCheckBox.Value;
                consumo_kWh = randi([30, 50]);
                if enfermedad
                    consumo_kWh = (randi([50, 150]) / 100) + consumo_kWh;
                end

                peso_corporal = app.PesocorporalEditField.Value;

                sumagases = 0;
                while sumagases>=0.98 && sumagases<= 1
                    oxido_nitroso = randi([30, 50]) / 100;
                    sevoflurano = randi([10, 20]) / 100;
                    oxigeno = randi([20, 40]) / 100;

                    sumagases = oxido_nitroso + sevoflurano + oxigeno;
                end

                assg = randi([90, 95]);

                duraciones = [randi([6, 12]), randi([3, 5]), randi([4, 6]), randi([50, 100]) / 100, randi([17, 50]) / 100, randi([34, 50]) / 100];

                porcentajes = [randi([20, 30]), randi([10, 20]), randi([40, 60])];
                sumacirugias = sum(porcentajes);

                cirugia_escogida = app.CirugiasListBox.Value;

                switch cirugia_escogida
                    case 'Transplante de higado'
                        gas_utilizado_hora = (randi([2, 4]) / 1000) * peso_corporal * duraciones(1);
                    case 'Transplante de riñon'
                        gas_utilizado_hora = (randi([2, 4]) / 1000) * peso_corporal * duraciones(2);
                    case 'Transplante de corazon'
                        gas_utilizado_hora = (randi([2, 4]) / 1000) * peso_corporal * duraciones(3);
                    case 'Extraccion de apendice'
                        gas_utilizado_hora = (randi([2, 4]) / 1000) * peso_corporal * duraciones(4);
                    case 'Cataratas'
                        gas_utilizado_hora = (randi([2, 4]) / 1000) * peso_corporal * duraciones(5);
                    case 'Tunel Carpiano'
                        gas_utilizado_hora = (randi([2, 4]) / 1000) * peso_corporal * duraciones(6);
                end

                energia_co2 = consumo_kWh * 164.38;

                cantidad_residuos = 2 * duraciones;
                biologico_co2 = cantidad_residuos * porcentajes(1) * 0.0002;
                cortopunzantesygenerales_co2 = sum(cantidad_residuos) * (porcentajes(2) + porcentajes(3)) * 0.6;

                oxido_nitroso_usado = gas_utilizado_hora * oxido_nitroso * 298;
                oxido_nitroso_retenido = oxido_nitroso_usado * (assg / 100);
                oxido_nitroso_liberado = oxido_nitroso_usado - oxido_nitroso_retenido;

                sevoflurano_usado = gas_utilizado_hora * sevoflurano * 130;
                sevoflurano_retenido = sevoflurano_usado * (assg / 100);
                sevoflurano_liberado = sevoflurano_usado - sevoflurano_retenido;

                co2_total = energia_co2 + biologico_co2 + cortopunzantesygenerales_co2 + oxido_nitroso_liberado + sevoflurano_liberado;


                co2_total_vec(i) = co2_total; 
                
            end
            plot(app.Grafica, co2_total_vec);
        end
    end


