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

    % Callbacks that handle component events
    methods (Access = private)

        % Button pushed function: GraficarButton
        function GraficarButtonPushed(app, event)
             function GraficarButtonPushed(app, event)
        GraficarPushed(app);
             end
        end
    end

    % Component initialization
    methods (Access = private)

        % Create UIFigure and components
        function createComponents(app)

            % Create UIFigure and hide until all components are created
            app.UIFigure = uifigure('Visible', 'off');
            app.UIFigure.Position = [100 100 669 589];
            app.UIFigure.Name = 'MATLAB App';

            % Create Grafica
            app.Grafica = uiaxes(app.UIFigure);
            title(app.Grafica, 'Huella de carbono')
            xlabel(app.Grafica, 'Iteraciones')
            ylabel(app.Grafica, 'Cantidad de CO2 liberado')
            zlabel(app.Grafica, 'Z')
            app.Grafica.Position = [137 88 381 225];

            % Create CirugiasListBoxLabel
            app.CirugiasListBoxLabel = uilabel(app.UIFigure);
            app.CirugiasListBoxLabel.BackgroundColor = [0.9412 0.9412 0.9412];
            app.CirugiasListBoxLabel.HorizontalAlignment = 'right';
            app.CirugiasListBoxLabel.Position = [80 438 49 22];
            app.CirugiasListBoxLabel.Text = 'Cirugias';

            % Create CirugiasListBox
            app.CirugiasListBox = uilistbox(app.UIFigure);
            app.CirugiasListBox.Items = {'Transplante de higado', 'Transplante de riñon', 'Transplante de corazon', 'Extraccion de apendice', 'Cataratas', 'Tunel Carpiano'};
            app.CirugiasListBox.BackgroundColor = [0.9412 0.9412 0.9412];
            app.CirugiasListBox.Position = [145 345 164 117];
            app.CirugiasListBox.Value = 'Transplante de higado';

            % Create EnfermedadCheckBox
            app.EnfermedadCheckBox = uicheckbox(app.UIFigure);
            app.EnfermedadCheckBox.Text = {'El paciente tiene alguna enfermedad '; '(pulmonar, cardiaca, etc)'};
            app.EnfermedadCheckBox.Position = [356 412 222 46];

            % Create GraficarButton
            app.GraficarButton = uibutton(app.UIFigure, 'push');
            app.GraficarButton.ButtonPushedFcn = createCallbackFcn(app, @GraficarButtonPushed, true);
            app.GraficarButton.WordWrap = 'on';
            app.GraficarButton.BackgroundColor = [1 0.7294 0.8784];
            app.GraficarButton.FontName = 'Comic Sans MS';
            app.GraficarButton.FontWeight = 'bold';
            app.GraficarButton.Position = [221 27 94 38];
            app.GraficarButton.Text = 'Graficar';

            % Create PesocorporalEditFieldLabel
            app.PesocorporalEditFieldLabel = uilabel(app.UIFigure);
            app.PesocorporalEditFieldLabel.HorizontalAlignment = 'right';
            app.PesocorporalEditFieldLabel.Position = [369 382 79 22];
            app.PesocorporalEditFieldLabel.Text = 'Peso corporal';

            % Create PesocorporalEditField
            app.PesocorporalEditField = uieditfield(app.UIFigure, 'numeric');
            app.PesocorporalEditField.Position = [463 382 100 22];

            % Create Titulo
            app.Titulo = uilabel(app.UIFigure);
            app.Titulo.HorizontalAlignment = 'center';
            app.Titulo.WordWrap = 'on';
            app.Titulo.FontName = 'Comic Sans MS';
            app.Titulo.FontSize = 18;
            app.Titulo.FontWeight = 'bold';
            app.Titulo.FontColor = [1 0.0745 0.651];
            app.Titulo.Position = [120 498 414 65];
            app.Titulo.Text = 'Impacto ambiental de las emisiones de CO2 en las salas de operación hospitalarias';

            % Create EstadisticoPromedio
            app.EstadisticoPromedio = uilabel(app.UIFigure);
            app.EstadisticoPromedio.Position = [379 345 63 22];
            app.EstadisticoPromedio.Text = 'Promedio: ';

            % Create Promedio
            app.Promedio = uilabel(app.UIFigure);
            app.Promedio.Position = [465 345 28 22];
            app.Promedio.Text = '5.94';

            % Create CalcularpromedioButton
            app.CalcularpromedioButton = uibutton(app.UIFigure, 'push');
            app.CalcularpromedioButton.WordWrap = 'on';
            app.CalcularpromedioButton.BackgroundColor = [1 0.7294 0.8784];
            app.CalcularpromedioButton.FontName = 'Comic Sans MS';
            app.CalcularpromedioButton.FontWeight = 'bold';
            app.CalcularpromedioButton.Position = [420 27 94 38];
            app.CalcularpromedioButton.Text = 'Calcular promedio';

            % Show the figure after all components are created
            app.UIFigure.Visible = 'on';
        end
    end

    % App creation and deletion
    methods (Access = public)

        % Construct app
        function app = app1

            % Create UIFigure and components
            createComponents(app)

            % Register the app with App Designer
            registerApp(app, app.UIFigure)

            if nargout == 0
                clear app
            end
        end

        % Code that executes before app deletion
        function delete(app)

            % Delete UIFigure when app is deleted
            delete(app.UIFigure)
        end
    end
end
