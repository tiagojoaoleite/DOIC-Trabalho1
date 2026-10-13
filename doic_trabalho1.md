# Trabalho 1 \- DOIC

# 1. Trânsito de Potências Determinístico
```matlab
% Matriz Ybus do sistema de 5 barras


Ybus = [
    4.92 - 13.85j, -4.11 + 10.96j,             0,             0, -0.81 +  2.90j;
    -4.11 + 10.96j, 11.51 - 25.64j, -5.62 + 8.99j,             0, -1.78 +  5.69j;
    0, -5.62 +  8.99j,  9.07 - 17.61j, -3.45 + 8.62j,             0;
    0,             0, -3.45 +  8.62j,  6.90 - 17.24j, -3.45 +  8.62j;
    -0.81 +  2.90j, -1.78 +  5.69j,             0, -3.45 + 8.62j,  6.04 - 17.21j
    ];


% Nomes dos barramentos (opcional, caso precise mapear os índices)
BusNames = {'ONE', 'TWO', 'THREE', 'FOUR', 'FIVE'};


disp_Ybus = array2table(Ybus, 'RowNames', BusNames, 'VariableNames', BusNames)
```

| |ONE|TWO|THREE|FOUR|FIVE|
|:--:|:--:|:--:|:--:|:--:|:--:|
|1 ONE|4.9200 -13.8500i|-4.1100 +10.9600i|0.0000 + 0.0000i|0.0000 + 0.0000i|-0.8100 + 2.9000i|
|2 TWO|-4.1100 +10.9600i|11.5100 -25.6400i|-5.6200 + 8.9900i|0.0000 + 0.0000i|-1.7800 + 5.6900i|
|3 THREE|0.0000 + 0.0000i|-5.6200 + 8.9900i|9.0700 -17.6100i|-3.4500 + 8.6200i|0.0000 + 0.0000i|
|4 FOUR|0.0000 + 0.0000i|0.0000 + 0.0000i|-3.4500 + 8.6200i|6.9000 -17.2400i|-3.4500 + 8.6200i|
|5 FIVE|-0.8100 + 2.9000i|-1.7800 + 5.6900i|0.0000 + 0.0000i|-3.4500 + 8.6200i|6.0400 -17.2100i|

# 2. Jacobiano e Inverso 
```matlab
% Matriz Jacobiana (10x10) do sistema de 5 barras
% Colunas 1-5: Derivadas em relação aos Ângulos (Ang1 a Ang5)
% Colunas 6-10: Derivadas em relação às Tensões (V1 a V5)


J = [
    1.00,      0,      0,      0,      0,   0,      0,      0,      0,      0;  % Bus 1: Real Power
    0,      0,      0,      0,      0, 1.00,     0,      0,      0,      0;     % Bus 1: Slack
    0, -11.49,   5.39,      0,   1.58,   0,  24.69,  -8.71,      0,  -5.56;     % Bus 2: Reactive Power
    0,  24.52,  -8.49,      0,  -5.32,   0,  10.58,  -5.52,      0,  -1.65;     % Bus 2: Real Power
    0,   5.27,  -9.03,   3.75,      0,   0,  -8.81,  17.02,  -8.26,      0;     % Bus 3: Reactive Power
    0,  -8.57,  16.91,  -8.34,      0,   0,  -5.42,   8.43,  -3.72,      0;     % Bus 3: Real Power
    0,      0,      0,      0,      0,   0,      0,      0,   1.00,      0;     % Bus 4: Voltage Magnitude
    0,      0,  -8.63,  17.16,  -8.53,   0,      0,  -3.11,   8.15,  -2.93;     % Bus 4: Real Power
    0,   1.73,      0,   3.85,  -6.53,   0,  -5.42,      0,  -8.02,  16.05;     % Bus 5: Reactive Power
    0,  -5.28,      0,  -8.10,  16.16,   0,  -1.78,      0,  -3.81,   4.74      % Bus 5: Real Power
    ];


Equacoes = {
    'Bus 1', 'Real Power';
    'Bus 1', 'Slack';
    'Bus 2', 'Reactive Power';
    'Bus 2', 'Real Power';
    'Bus 3', 'Reactive Power';
    'Bus 3', 'Real Power';
    'Bus 4', 'Voltage Magnitude';
    'Bus 4', 'Real Power';
    'Bus 5', 'Reactive Power';
    'Bus 5', 'Real Power'
    };


ln  = [4 6 8 10 3 5 9];     % P2 P3 P4 P5 Q2 Q3 Q5
col = [2 3 4 5 7 8 10];     % θ2 θ3 θ4 θ5 V2 V3 V5


Jr = J(ln,col);


Rows =        {'P2'  ,'P3'  ,'P4'  ,'P5'  ,'Q2', 'Q3', 'Q5'};
Variables =   {'Ang2','Ang3','Ang4','Ang5','V2', 'V3', 'V5'};


disp_Jr = array2table(Jr, 'RowNames', Rows, 'VariableNames', Variables)
```

| |Ang2|Ang3|Ang4|Ang5|V2|V3|V5|
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|1 P2|24.5200|-8.4900|0|-5.3200|10.5800|-5.5200|-1.6500|
|2 P3|-8.5700|16.9100|-8.3400|0|-5.4200|8.4300|0|
|3 P4|0|-8.6300|17.1600|-8.5300|0|-3.1100|-2.9300|
|4 P5|-5.2800|0|-8.1000|16.1600|-1.7800|0|4.7400|
|5 Q2|-11.4900|5.3900|0|1.5800|24.6900|-8.7100|-5.5600|
|6 Q3|5.2700|-9.0300|3.7500|0|-8.8100|17.0200|0|
|7 Q5|1.7300|0|3.8500|-6.5300|-5.4200|0|16.0500|

```matlab
Ji = inv(Jr)
```

```matlabTextOutput
Ji = 7x7
    0.0721    0.0692    0.0628    0.0571   -0.0147   -0.0070   -0.0031
    0.0706    0.1385    0.1121    0.0835   -0.0029   -0.0267    0.0020
    0.0670    0.1157    0.1665    0.1119    0.0052   -0.0025    0.0060
    0.0575    0.0813    0.1051    0.1308   -0.0016   -0.0032   -0.0141
    0.0195    0.0076   -0.0023    0.0070    0.0472    0.0263    0.0159
    0.0105    0.0305    0.0022    0.0056    0.0263    0.0609    0.0089
    0.0061    0.0004   -0.0047    0.0226    0.0156    0.0089    0.0608


```

```matlab
J_pos = Ji.*(Ji>0)
```

```matlabTextOutput
J_pos = 7x7
    0.0721    0.0692    0.0628    0.0571         0         0         0
    0.0706    0.1385    0.1121    0.0835         0         0    0.0020
    0.0670    0.1157    0.1665    0.1119    0.0052         0    0.0060
    0.0575    0.0813    0.1051    0.1308         0         0         0
    0.0195    0.0076         0    0.0070    0.0472    0.0263    0.0159
    0.0105    0.0305    0.0022    0.0056    0.0263    0.0609    0.0089
    0.0061    0.0004         0    0.0226    0.0156    0.0089    0.0608


```

```matlab
J_neg = Ji.*(Ji<0)
```

```matlabTextOutput
J_neg = 7x7
         0         0         0         0   -0.0147   -0.0070   -0.0031
         0         0         0         0   -0.0029   -0.0267         0
         0         0         0         0         0   -0.0025         0
         0         0         0         0   -0.0016   -0.0032   -0.0141
         0         0   -0.0023         0         0         0         0
         0         0         0         0         0         0         0
         0         0   -0.0047         0         0         0         0


```

# 3. Desvios das Potências Injetadas em relação ao valor central
```matlab
dZa = [-7 -5 -8 -6 -5 -2 -6]'/100;           % limites inferiores (p.u.)    P2 P3 P4 P5 Q2 Q3 Q5
dZc = [ 6  2 10  5  5  3  5]'/100;           % limites superiores (p.u.)    P2 P3 P4 P5 Q2 Q3 Q5
```

# 4. Variação dos Ângulos e do modulo das Tensões
```matlab
dXa = J_pos*dZa + J_neg*dZc;
dXc = J_pos*dZc + J_neg*dZa;


```

# 5. Intervalo de Possibilidades para os Ângulos e para o mudulo das Tensões
```matlab
X0 = [deg2rad([-2.347 -1.963 0.462 -3.158]) 0.9728 0.9755 0.9569]';


Xa = X0 + dXa;  
Xc = X0 + dXc;


ang = rad2deg([Xa(1:4) X0(1:4) Xc(1:4)]);


ln_ang  = {'ang2', 'ang3', 'ang4', 'ang5'};
col_lim = {'a', 'b', 'c'};


disp_ang = array2table(ang, 'RowNames', ln_ang, 'VariableNames', col_lim)
```

| |a|b|c|
|:--:|:--:|:--:|:--:|
|1 ang2|-3.3812|-2.3470|-1.4360|
|2 ang3|-3.5052|-1.9630|-0.6351|
|3 ang4|-1.3258|0.4620|2.1343|
|4 ang5|-4.6032|-3.1580|-1.8338|

```matlab
V   = [Xa(5:7) X0(5:7) Xc(5:7)];             % a, b, c em p.u


ln_v  = {'V2', 'V3','V5'};
% col_lim = {'a', 'b', 'c'};


disp_v = array2table(V, 'RowNames', ln_v, 'VariableNames', col_lim)
```

| |a|b|c|
|:--:|:--:|:--:|:--:|
|1 V2|0.9666|0.9728|0.9786|
|2 V3|0.9697|0.9755|0.9808|
|3 V5|0.9500|0.9569|0.9629|

# 6. Sensibilidades da potência ativa nas linhas (sentido i\->k)
```matlab
% Variáveis de estado: x  = [θ2 θ3 θ4 θ5 V2 V3 V5]
% Injeções:            ΔZ = [P2 P3 P4 P5 Q2 Q3 Q5]


% P_ik = S_ik * dZ      θ2 θ3 θ4 θ5 V2 V3 V5
% S_ik = ∂P_ik/∂x
% S_12 = [∂P_12/∂θ2 0 0 0 ∂P_12/∂V2 0 0]
% S_15 = [0 0 0 ∂P_15/∂θ5 0 0 ∂P_15/∂V5]
% S_23 = [∂P_23/∂θ2 ∂P_23/∂θ3 0 0 ∂P_23/∂V2 ∂P_23/∂V3 0]
% S_25 = [∂P_25/∂θ2 0 0 ∂P_25/∂θ5 ∂P_25/∂V2 0 ∂P_25/∂V5]
% S_34 = [0 ∂P_34/∂θ3 ∂P_34/∂θ4 0 0 ∂P_34/∂V3 0]
% S_45 = [0 0 ∂P_45/∂θ4 ∂P_45/∂θ5 0 0 ∂P_45/∂V5]


% P_ik = g·V_i² − V_i·V_k·(g·cos θ_ik + b·sin θ_ik), com θ_ik = θ_i − θ_k


% ∂P_ik/∂θ_i = V_i·V_k·(g·sin θ_ik − b·cos θ_ik)
% ∂P_ik/∂θ_k = −V_i·V_k·(g·sin θ_ik − b·cos θ_ik)
% ∂P_ik/∂V_i = 2·g·V_i − V_k·(g·cos θ_ik + b·sin θ_ik)
% ∂P_ik/∂V_k = −V_i·(g·cos θ_ik + b·sin θ_ik)


%% Ponto de funcionamento central (PowerWorld, Pg4 = 150 MW)
V  = [1.0200 0.9728 0.9755 1.0100 0.9569];              % p.u.
th = deg2rad([0 -2.3472 -1.9630 0.4621 -3.1577]);       % rad


%% Admitâncias série y = 1/(R+jX) = g + jb
y12 = - Ybus(1,2);  g12 = - real(Ybus(1,2));  b12 = - imag(Ybus(1,2));
y15 = - Ybus(1,5);  g15 = - real(Ybus(1,5));  b15 = - imag(Ybus(1,5));
y23 = - Ybus(2,3);  g23 = - real(Ybus(2,3));  b23 = - imag(Ybus(2,3));
y25 = - Ybus(2,5);  g25 = - real(Ybus(2,5));  b25 = - imag(Ybus(2,5));
y34 = - Ybus(3,4);  g34 = - real(Ybus(3,4));  b34 = - imag(Ybus(3,4));
y45 = - Ybus(4,5);  g45 = - real(Ybus(4,5));  b45 = - imag(Ybus(4,5));


%% Diferenças angulares θik = θi - θk
t12 = th(1)-th(2);
t15 = th(1)-th(5);
t23 = th(2)-th(3);
t25 = th(2)-th(5);
t34 = th(3)-th(4);
t45 = th(4)-th(5);


%% Linha 1-2  (θ1, V1 fixos)
dP12_dth2 = -V(1)*V(2)*(g12*sin(t12) - b12*cos(t12));
dP12_dV2  = -V(1)*(g12*cos(t12) + b12*sin(t12));


%% Linha 1-5  (θ1, V1 fixos)
dP15_dth5 = -V(1)*V(5)*(g15*sin(t15) - b15*cos(t15));
dP15_dV5  = -V(1)*(g15*cos(t15) + b15*sin(t15));


%% Linha 2-3
dP23_dth2 =  V(2)*V(3)*(g23*sin(t23) - b23*cos(t23));
dP23_dth3 = -V(2)*V(3)*(g23*sin(t23) - b23*cos(t23));
dP23_dV2  =  2*g23*V(2) - V(3)*(g23*cos(t23) + b23*sin(t23));
dP23_dV3  = -V(2)*(g23*cos(t23) + b23*sin(t23));


%% Linha 2-5
dP25_dth2 =  V(2)*V(5)*(g25*sin(t25) - b25*cos(t25));
dP25_dth5 = -V(2)*V(5)*(g25*sin(t25) - b25*cos(t25));
dP25_dV2  =  2*g25*V(2) - V(5)*(g25*cos(t25) + b25*sin(t25));
dP25_dV5  = -V(2)*(g25*cos(t25) + b25*sin(t25));


%% Linha 3-4  (V4 fixo)
dP34_dth3 =  V(3)*V(4)*(g34*sin(t34) - b34*cos(t34));
dP34_dth4 = -V(3)*V(4)*(g34*sin(t34) - b34*cos(t34));
dP34_dV3  =  2*g34*V(3) - V(4)*(g34*cos(t34) + b34*sin(t34));


%% Linha 4-5  (V4 fixo)
dP45_dth4 =  V(4)*V(5)*(g45*sin(t45) - b45*cos(t45));
dP45_dth5 = -V(4)*V(5)*(g45*sin(t45) - b45*cos(t45));
dP45_dV5  = -V(4)*(g45*cos(t45) + b45*sin(t45));


%% Matriz das derivadas ∂P_ik/∂x  (6 linhas x 7 variáveis)
%         θ2         θ3         θ4         θ5         V2        V3        V5
dP = [dP12_dth2  0          0          0          dP12_dV2  0         0        ;   % 1-2
      0          0          0          dP15_dth5  0         0         dP15_dV5 ;   % 1-5
      dP23_dth2  dP23_dth3  0          0          dP23_dV2  dP23_dV3  0        ;   % 2-3
      dP25_dth2  0          0          dP25_dth5  dP25_dV2  0         dP25_dV5 ;   % 2-5
      0          dP34_dth3  dP34_dth4  0          0         dP34_dV3  0        ;   % 3-4
      0          0          dP45_dth4  dP45_dth5  0         0         dP45_dV5];   % 4-5


%% Matriz de sensibilidades [S_Pik] = [∂P_ik/∂x] * J^-1
SP  = dP * Ji;
SP_pos = SP.*(SP>0);
SP_neg = SP.*(SP<0);


%% Desvios difusos (p.u.)
dPa = SP_pos*dZa + SP_neg*dZc;
dPc = SP_pos*dZc + SP_neg*dZa;


dP_pu = [dPa dPc]
```

```matlabTextOutput
dP_pu = 6x2
   -0.1717    0.1970
   -0.0645    0.0712
   -0.0754    0.0894
   -0.0526    0.0531
   -0.0754    0.0596
   -0.0749    0.0784


```

```matlab


%% Trânsito difuso = central + desvios (MW)
P0 = 100*[g12*V(1)^2 - V(1)*V(2)*(g12*cos(t12) + b12*sin(t12));
          g15*V(1)^2 - V(1)*V(5)*(g15*cos(t15) + b15*sin(t15));
          g23*V(2)^2 - V(2)*V(3)*(g23*cos(t23) + b23*sin(t23));
          g25*V(2)^2 - V(2)*V(5)*(g25*cos(t25) + b25*sin(t25));
          g34*V(3)^2 - V(3)*V(4)*(g34*cos(t34) + b34*sin(t34));
          g45*V(4)^2 - V(4)*V(5)*(g45*cos(t45) + b45*sin(t45))];


Pfuzzy = [P0 + 100*dPa, P0, P0 + 100*dPc]      % colunas a | b | c
```

```matlabTextOutput
Pfuzzy = 6x3
   47.5028   64.6683   84.3688
   14.4718   20.9250   28.0446
  -14.7215   -7.1847    1.7521
    5.0033   10.2622   15.5711
  -54.7876  -47.2428  -41.2861
   64.2791   71.7658   79.6024


```

# 7. Trânsitos difusos P\_ki, Q\_ik e Q\_ki
```matlab


% Variáveis de estado: x  = [θ2 θ3 θ4 θ5 V2 V3 V5]
% Injeções:            ΔZ = [P2 P3 P4 P5 Q2 Q3 Q5]
%
% Expressões (θki = θk - θi = -θik):
%   P_ki = g Vk^2 - Vk Vi (g cos θki + b sin θki)
%   Q_ik = -b Vi^2 - Vi Vk (g sin θik - b cos θik)
%   Q_ki = -b Vk^2 - Vk Vi (g sin θki - b cos θki)


%% Diferenças angulares no sentido inverso θki = θk - θi
t21 = th(2)-th(1);
t51 = th(5)-th(1);
t32 = th(3)-th(2);
t52 = th(5)-th(2);
t43 = th(4)-th(3);
t54 = th(5)-th(4);


%% ======================= P_ki =======================
% dP_ki/dθk =  Vk Vi (g sin θki - b cos θki)
% dP_ki/dθi = -Vk Vi (g sin θki - b cos θki)
% dP_ki/dVk =  2 g Vk - Vi (g cos θki + b sin θki)
% dP_ki/dVi = -Vk (g cos θki + b sin θki)


% Linha 2-1  (θ1, V1 fixos)
dP21_dth2 =  V(2)*V(1)*(g12*sin(t21) - b12*cos(t21));
dP21_dV2  =  2*g12*V(2) - V(1)*(g12*cos(t21) + b12*sin(t21));


% Linha 5-1  (θ1, V1 fixos)
dP51_dth5 =  V(5)*V(1)*(g15*sin(t51) - b15*cos(t51));
dP51_dV5  =  2*g15*V(5) - V(1)*(g15*cos(t51) + b15*sin(t51));


% Linha 3-2
dP32_dth3 =  V(3)*V(2)*(g23*sin(t32) - b23*cos(t32));
dP32_dth2 = -V(3)*V(2)*(g23*sin(t32) - b23*cos(t32));
dP32_dV3  =  2*g23*V(3) - V(2)*(g23*cos(t32) + b23*sin(t32));
dP32_dV2  = -V(3)*(g23*cos(t32) + b23*sin(t32));


% Linha 5-2
dP52_dth5 =  V(5)*V(2)*(g25*sin(t52) - b25*cos(t52));
dP52_dth2 = -V(5)*V(2)*(g25*sin(t52) - b25*cos(t52));
dP52_dV5  =  2*g25*V(5) - V(2)*(g25*cos(t52) + b25*sin(t52));
dP52_dV2  = -V(5)*(g25*cos(t52) + b25*sin(t52));


% Linha 4-3  (V4 fixo)
dP43_dth4 =  V(4)*V(3)*(g34*sin(t43) - b34*cos(t43));
dP43_dth3 = -V(4)*V(3)*(g34*sin(t43) - b34*cos(t43));
dP43_dV3  = -V(4)*(g34*cos(t43) + b34*sin(t43));


% Linha 5-4  (V4 fixo)
dP54_dth5 =  V(5)*V(4)*(g45*sin(t54) - b45*cos(t54));
dP54_dth4 = -V(5)*V(4)*(g45*sin(t54) - b45*cos(t54));
dP54_dV5  =  2*g45*V(5) - V(4)*(g45*cos(t54) + b45*sin(t54));


%          θ2         θ3         θ4         θ5         V2        V3        V5
dPki = [dP21_dth2  0          0          0          dP21_dV2  0         0       ;   % 2-1
        0          0          0          dP51_dth5  0         0         dP51_dV5;   % 5-1
        dP32_dth2  dP32_dth3  0          0          dP32_dV2  dP32_dV3  0       ;   % 3-2
        dP52_dth2  0          0          dP52_dth5  dP52_dV2  0         dP52_dV5;   % 5-2
        0          dP43_dth3  dP43_dth4  0          0         dP43_dV3  0       ;   % 4-3
        0          0          dP54_dth4  dP54_dth5  0         0         dP54_dV5];  % 5-4


%% ======================= Q_ik =======================
% dQ_ik/dθi = -Vi Vk (g cos θik + b sin θik)
% dQ_ik/dθk =  Vi Vk (g cos θik + b sin θik)
% dQ_ik/dVi = -2 b Vi - Vk (g sin θik - b cos θik)
% dQ_ik/dVk = -Vi (g sin θik - b cos θik)


% Linha 1-2  (θ1, V1 fixos)
dQ12_dth2 =  V(1)*V(2)*(g12*cos(t12) + b12*sin(t12));
dQ12_dV2  = -V(1)*(g12*sin(t12) - b12*cos(t12));


% Linha 1-5  (θ1, V1 fixos)
dQ15_dth5 =  V(1)*V(5)*(g15*cos(t15) + b15*sin(t15));
dQ15_dV5  = -V(1)*(g15*sin(t15) - b15*cos(t15));


% Linha 2-3
dQ23_dth2 = -V(2)*V(3)*(g23*cos(t23) + b23*sin(t23));
dQ23_dth3 =  V(2)*V(3)*(g23*cos(t23) + b23*sin(t23));
dQ23_dV2  = -2*b23*V(2) - V(3)*(g23*sin(t23) - b23*cos(t23));
dQ23_dV3  = -V(2)*(g23*sin(t23) - b23*cos(t23));


% Linha 2-5
dQ25_dth2 = -V(2)*V(5)*(g25*cos(t25) + b25*sin(t25));
dQ25_dth5 =  V(2)*V(5)*(g25*cos(t25) + b25*sin(t25));
dQ25_dV2  = -2*b25*V(2) - V(5)*(g25*sin(t25) - b25*cos(t25));
dQ25_dV5  = -V(2)*(g25*sin(t25) - b25*cos(t25));


% Linha 3-4  (V4 fixo)
dQ34_dth3 = -V(3)*V(4)*(g34*cos(t34) + b34*sin(t34));
dQ34_dth4 =  V(3)*V(4)*(g34*cos(t34) + b34*sin(t34));
dQ34_dV3  = -2*b34*V(3) - V(4)*(g34*sin(t34) - b34*cos(t34));


% Linha 4-5  (V4 fixo)
dQ45_dth4 = -V(4)*V(5)*(g45*cos(t45) + b45*sin(t45));
dQ45_dth5 =  V(4)*V(5)*(g45*cos(t45) + b45*sin(t45));
dQ45_dV5  = -V(4)*(g45*sin(t45) - b45*cos(t45));


%          θ2         θ3         θ4         θ5         V2        V3        V5
dQik = [dQ12_dth2  0          0          0          dQ12_dV2  0         0       ;   % 1-2
        0          0          0          dQ15_dth5  0         0         dQ15_dV5;   % 1-5
        dQ23_dth2  dQ23_dth3  0          0          dQ23_dV2  dQ23_dV3  0       ;   % 2-3
        dQ25_dth2  0          0          dQ25_dth5  dQ25_dV2  0         dQ25_dV5;   % 2-5
        0          dQ34_dth3  dQ34_dth4  0          0         dQ34_dV3  0       ;   % 3-4
        0          0          dQ45_dth4  dQ45_dth5  0         0         dQ45_dV5];  % 4-5


%% ======================= Q_ki =======================
% dQ_ki/dθk = -Vk Vi (g cos θki + b sin θki)
% dQ_ki/dθi =  Vk Vi (g cos θki + b sin θki)
% dQ_ki/dVk = -2 b Vk - Vi (g sin θki - b cos θki)
% dQ_ki/dVi = -Vk (g sin θki - b cos θki)


% Linha 2-1  
dQ21_dth2 = -V(2)*V(1)*(g12*cos(t21) + b12*sin(t21));
dQ21_dV2  = -2*b12*V(2) - V(1)*(g12*sin(t21) - b12*cos(t21));


% Linha 5-1  
dQ51_dth5 = -V(5)*V(1)*(g15*cos(t51) + b15*sin(t51));
dQ51_dV5  = -2*b15*V(5) - V(1)*(g15*sin(t51) - b15*cos(t51));


% Linha 3-2
dQ32_dth3 = -V(3)*V(2)*(g23*cos(t32) + b23*sin(t32));
dQ32_dth2 =  V(3)*V(2)*(g23*cos(t32) + b23*sin(t32));
dQ32_dV3  = -2*b23*V(3) - V(2)*(g23*sin(t32) - b23*cos(t32));
dQ32_dV2  = -V(3)*(g23*sin(t32) - b23*cos(t32));


% Linha 5-2
dQ52_dth5 = -V(5)*V(2)*(g25*cos(t52) + b25*sin(t52));
dQ52_dth2 =  V(5)*V(2)*(g25*cos(t52) + b25*sin(t52));
dQ52_dV5  = -2*b25*V(5) - V(2)*(g25*sin(t52) - b25*cos(t52));
dQ52_dV2  = -V(5)*(g25*sin(t52) - b25*cos(t52));


% Linha 4-3  (V4 fixo)
dQ43_dth4 = -V(4)*V(3)*(g34*cos(t43) + b34*sin(t43));
dQ43_dth3 =  V(4)*V(3)*(g34*cos(t43) + b34*sin(t43));
dQ43_dV3  = -V(4)*(g34*sin(t43) - b34*cos(t43));


% Linha 5-4  (V4 fixo)
dQ54_dth5 = -V(5)*V(4)*(g45*cos(t54) + b45*sin(t54));
dQ54_dth4 =  V(5)*V(4)*(g45*cos(t54) + b45*sin(t54));
dQ54_dV5  = -2*b45*V(5) - V(4)*(g45*sin(t54) - b45*cos(t54));


%          θ2         θ3         θ4         θ5         V2        V3        V5
dQki = [dQ21_dth2  0          0          0          dQ21_dV2  0         0       ;   % 2-1
        0          0          0          dQ51_dth5  0         0         dQ51_dV5;   % 5-1
        dQ32_dth2  dQ32_dth3  0          0          dQ32_dV2  dQ32_dV3  0       ;   % 3-2
        dQ52_dth2  0          0          dQ52_dth5  dQ52_dV2  0         dQ52_dV5;   % 5-2
        0          dQ43_dth3  dQ43_dth4  0          0         dQ43_dV3  0       ;   % 4-3
        0          0          dQ54_dth4  dQ54_dth5  0         0         dQ54_dV5];  % 5-4


%% ============ Matrizes de sensibilidade e desvios difusos ============
SPki = dPki*Ji;   SQik = dQik*Ji;   SQki = dQki*Ji;


fuzzy = @(S) [S.*(S>0)*dZa + S.*(S<0)*dZc, ...     % desvio inferior (a)
              S.*(S>0)*dZc + S.*(S<0)*dZa];        % desvio superior (c)


dPki_ac = fuzzy(SPki);
dQik_ac = fuzzy(SQik);
dQki_ac = fuzzy(SQki);


%% ============ Valores centrais (fórmulas) — devem bater com o PowerWorld ============
Pki0 = 100*[g12*V(2)^2 - V(2)*V(1)*(g12*cos(t21) + b12*sin(t21));
            g15*V(5)^2 - V(5)*V(1)*(g15*cos(t51) + b15*sin(t51));
            g23*V(3)^2 - V(3)*V(2)*(g23*cos(t32) + b23*sin(t32));
            g25*V(5)^2 - V(5)*V(2)*(g25*cos(t52) + b25*sin(t52));
            g34*V(4)^2 - V(4)*V(3)*(g34*cos(t43) + b34*sin(t43));
            g45*V(5)^2 - V(5)*V(4)*(g45*cos(t54) + b45*sin(t54))];


Qik0 = 100*[-b12*V(1)^2 - V(1)*V(2)*(g12*sin(t12) - b12*cos(t12));
            -b15*V(1)^2 - V(1)*V(5)*(g15*sin(t15) - b15*cos(t15));
            -b23*V(2)^2 - V(2)*V(3)*(g23*sin(t23) - b23*cos(t23));
            -b25*V(2)^2 - V(2)*V(5)*(g25*sin(t25) - b25*cos(t25));
            -b34*V(3)^2 - V(3)*V(4)*(g34*sin(t34) - b34*cos(t34));
            -b45*V(4)^2 - V(4)*V(5)*(g45*sin(t45) - b45*cos(t45))];


Qki0 = 100*[-b12*V(2)^2 - V(2)*V(1)*(g12*sin(t21) - b12*cos(t21));
            -b15*V(5)^2 - V(5)*V(1)*(g15*sin(t51) - b15*cos(t51));
            -b23*V(3)^2 - V(3)*V(2)*(g23*sin(t32) - b23*cos(t32));
            -b25*V(5)^2 - V(5)*V(2)*(g25*sin(t52) - b25*cos(t52));
            -b34*V(4)^2 - V(4)*V(3)*(g34*sin(t43) - b34*cos(t43));
            -b45*V(5)^2 - V(5)*V(4)*(g45*sin(t54) - b45*cos(t54))];


%% ============ Resultados difusos (MW / Mvar): colunas a | b | c ============
Pki_fuzzy = [Pki0 + 100*dPki_ac(:,1), Pki0, Pki0 + 100*dPki_ac(:,2)];
Qik_fuzzy = [Qik0 + 100*dQik_ac(:,1), Qik0, Qik0 + 100*dQik_ac(:,2)];
Qki_fuzzy = [Qki0 + 100*dQki_ac(:,1), Qki0, Qki0 + 100*dQki_ac(:,2)];


%% ============ Tabela resumo ============
Linha = {'1-2';'1-5';'2-3';'2-5';'3-4';'4-5'};
T = table(Linha, Pfuzzy, Pki_fuzzy, Qik_fuzzy, Qki_fuzzy, ...
    'VariableNames', {'Linha','Pik_abc','Pki_abc','Qik_abc','Qki_abc'})
```

| |Linha|Pik_abc| | |Pki_abc| | |Qik_abc| | |Qki_abc| | |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| | |1|2|3|1|2|3|1|2|3|1|2|3|
|1|'1-2'|47.5028|64.6683|84.3688|-82.0163|-63.0684|-46.5669|28.2100|36.9761|45.4561|-42.3631|-32.7096|-22.5341|
|2|'1-5'|14.4718|20.9250|28.0446|-27.2747|-20.3625|-14.0891|12.3443|14.7398|17.2757|-15.8547|-12.7256|-9.6386|
|3|'2-3'|-14.7215|-7.1847|1.7521|-1.7942|7.2128|14.8081|-5.8808|1.2341|7.5568|-7.4197|-1.1892|5.8149|
|4|'2-5'|5.0033|10.2622|15.5711|-15.4342|-10.1840|-4.9814|2.4909|6.5102|10.7998|-10.5860|-6.2603|-2.1970|
|5|'3-4'|-54.7876|-47.2428|-41.2861|42.0964|48.2622|56.0779|-20.8688|-13.8669|-6.6409|9.4197|16.4142|23.3383|
|6|'4-5'|64.2791|71.7658|79.6024|-76.8884|-69.4626|-62.3505|20.0839|26.8406|34.3846|-28.9111|-21.0860|-13.9576|

# 8. Perdas difusas nas linhas
```matlab
%   dP   (6x7)  derivadas de P_ik em ordem a x   (passo 6)
%   dPki (6x7)  derivadas de P_ki                  (passo 8)
%   dQik (6x7)  derivadas de Q_ik                  (passo 8)
%   dQki (6x7)  derivadas de Q_ki                  (passo 8)
%   P0, Pki0, Qik0, Qki0  valores centrais em MW / Mvar
%   Ji = inv(Jr), dZa, dZc
%
% Variáveis de estado: x  = [θ2 θ3 θ4 θ5 V2 V3 V5]
% Injeções:            ΔZ = [P2 P3 P4 P5 Q2 Q3 Q5]
%
% Ideia: as perdas de uma linha são  P_perdas = P_ik + P_ki
%                                    Q_perdas = Q_ik + Q_ki
% Como P_ik e P_ki dependem das MESMAS injeções ΔZ, a sensibilidade das
% perdas é a SOMA ALGÉBRICA das matrizes de sensibilidade:
%       [S_Pperdas] = [S_Pik] + [S_Pki]
% Só DEPOIS de somar é que se separa em parte positiva e negativa.
% (Somar os intervalos difusos de P_ik e P_ki já calculados seria errado:
%  trataria P_ik e P_ki como independentes e daria intervalos enormes,
%  da ordem de ±15 MW, sem significado físico.)


% 1) Matrizes de sensibilidade (6 linhas x 7 injeções)
SPik = dP   * Ji;            % do passo 6 (é a matriz SP)
SPki = dPki * Ji;            % do passo 8
SQik = dQik * Ji;
SQki = dQki * Ji;


SPloss = SPik + SPki;        % sensibilidade das perdas ativas
SQloss = SQik + SQki;        % sensibilidade das perdas reativas


% 2) Separação em parte positiva e negativa e desvios difusos (p.u.)
% Limite inferior: coeficientes positivos com ΔZa, negativos com ΔZc
% Limite superior: coeficientes positivos com ΔZc, negativos com ΔZa
SPp = SPloss.*(SPloss>0);   SPn = SPloss.*(SPloss<0);
SQp = SQloss.*(SQloss>0);   SQn = SQloss.*(SQloss<0);


dPloss_a = SPp*dZa + SPn*dZc;
dPloss_c = SPp*dZc + SPn*dZa;
dQloss_a = SQp*dZa + SQn*dZc;
dQloss_c = SQp*dZc + SQn*dZa;


% 3) Valores centrais das perdas (MW / Mvar)
% Devem coincidir com as colunas "MW Loss" e "Mvar Loss" do PowerWorld
Ploss0 = P0   + Pki0;
Qloss0 = Qik0 + Qki0;


% 4) Perdas difusas por linha: colunas a | b | c
Ploss_fuzzy = [Ploss0 + 100*dPloss_a, Ploss0, Ploss0 + 100*dPloss_c]
```

```matlabTextOutput
Ploss_fuzzy = 6x3
    0.9359    1.6000    2.3525
    0.3265    0.5626    0.8262
   -0.0460    0.0281    0.0905
    0.0100    0.0782    0.1488
    0.7644    1.0195    1.3362
    1.8325    2.3032    2.8102


```

```matlab
Qloss_fuzzy = [Qloss0 + 100*dQloss_a, Qloss0, Qloss0 + 100*dQloss_c]
```

```matlabTextOutput
Qloss_fuzzy = 6x3
    2.4957    4.2666    6.2733
    1.1689    2.0142    2.9579
   -0.0736    0.0449    0.1448
    0.0319    0.2498    0.4757
    1.9098    2.5473    3.3385
    4.5785    5.7546    7.0214


```

```matlab


% 5) Perdas totais do sistema
% Mesmo princípio: somam-se as linhas das matrizes de sensibilidade
% (uma única linha 1x7) e só depois se separa em +/-.
ST_P = sum(SPloss,1);   ST_Q = sum(SQloss,1);


PT0 = sum(Ploss0);      QT0 = sum(Qloss0);
PT_fuzzy = [PT0 + 100*(ST_P.*(ST_P>0)*dZa + ST_P.*(ST_P<0)*dZc), PT0, ...
    PT0 + 100*(ST_P.*(ST_P>0)*dZc + ST_P.*(ST_P<0)*dZa)]
```

```matlabTextOutput
PT_fuzzy = 1x3
    4.6731    5.5915    6.7144


```

```matlab
QT_fuzzy = [QT0 + 100*(ST_Q.*(ST_Q>0)*dZa + ST_Q.*(ST_Q<0)*dZc), QT0, ...
    QT0 + 100*(ST_Q.*(ST_Q>0)*dZc + ST_Q.*(ST_Q<0)*dZa)]
```

```matlabTextOutput
QT_fuzzy = 1x3
   12.4547   14.8774   17.8681


```

```matlab


% 6) Deteção de perdas ativas negativas (prepara o passo 11)
Linha = {'1-2';'1-5';'2-3';'2-5';'3-4';'4-5'};
neg = find(Ploss_fuzzy(:,1) < 0);
for n = neg'
    % Combinação de extremos que, pela matriz, dá o mínimo das perdas:
    % coeficiente > 0  -> injeção no limite inferior (ΔZa)
    % coeficiente < 0  -> injeção no limite superior (ΔZc)
    s  = SPloss(n,:)';
    dZmin = dZa.*(s>0) + dZc.*(s<=0);
    fprintf('Linha %s: perdas mínimas = %.3f MW (negativas!)\n', ...
        Linha{n}, Ploss_fuzzy(n,1));
    fprintf('  Combinação de ΔZ a usar no PowerWorld (MW/Mvar):\n');
    disp(array2table(100*dZmin', 'VariableNames', ...
        {'dP2','dP3','dP4','dP5','dQ2','dQ3','dQ5'}))
end
```

```matlabTextOutput
Linha 2-3: perdas mínimas = -0.046 MW (negativas!)
  Combinação de ΔZ a usar no PowerWorld (MW/Mvar):
    dP2    dP3    dP4    dP5    dQ2    dQ3    dQ5
    ___    ___    ___    ___    ___    ___    ___


     6     -5     -8     -6     -5      3     -6 
```

```matlab
% 7) Tabela resumo
T9 = table(Linha, Ploss_fuzzy, Qloss_fuzzy, ...
    'VariableNames', {'Linha','Pperdas_abc_MW','Qperdas_abc_Mvar'})
```

| |Linha|Pperdas_abc_MW| | |Qperdas_abc_Mvar| | |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| | |1|2|3|1|2|3|
|1|'1-2'|0.9359|1.6000|2.3525|2.4957|4.2666|6.2733|
|2|'1-5'|0.3265|0.5626|0.8262|1.1689|2.0142|2.9579|
|3|'2-3'|-0.0460|0.0281|0.0905|-0.0736|0.0449|0.1448|
|4|'2-5'|0.0100|0.0782|0.1488|0.0319|0.2498|0.4757|
|5|'3-4'|0.7644|1.0195|1.3362|1.9098|2.5473|3.3385|
|6|'4-5'|1.8325|2.3032|2.8102|4.5785|5.7546|7.0214|

# 9. Trânsito de potências difuso DC
```matlab


X  = [0.08 0.32 0.08 0.16 0.10 0.10];          % 1-2 1-5 2-3 2-5 3-4 4-5
de = [1 1 2 2 3 4];  pa = [2 5 3 5 4 5];       % nós de origem / destino


B = zeros(5);
for l = 1:6
    i = de(l); k = pa(l);
    B(i,i) = B(i,i) + 1/X(l);   B(k,k) = B(k,k) + 1/X(l);
    B(i,k) = B(i,k) - 1/X(l);   B(k,i) = B(k,i) - 1/X(l);
end
Br  = B(2:5,2:5);            % retira o slack (nó 1)
Bi  = inv(Br);


P0  = [-60 -40 120 -100]'/100;               % injeções centrais P2..P5
dPa = dZa(1:4);  dPc = dZc(1:4);             % só a parte ativa de ΔZ


% Ângulos difusos
th0 = Bi*P0;
tha = th0 + Bi.*(Bi>0)*dPa + Bi.*(Bi<0)*dPc;
thc = th0 + Bi.*(Bi>0)*dPc + Bi.*(Bi<0)*dPa;


ang_DC = rad2deg([tha th0 thc]);


ln_DC  = {'ang2', 'ang3','ang4', 'ang5'};
col_DC = {'a', 'b', 'c'};


disp_ang_DC = array2table(ang_DC, 'RowNames', ln_DC, 'VariableNames', col_DC)
```

| |a|b|c|
|:--:|:--:|:--:|:--:|
|1 ang2|-3.6483|-2.7635|-1.9889|
|2 ang3|-3.6291|-2.3516|-1.2495|
|3 ang4|-1.0267|0.4550|1.8519|
|4 ang5|-4.8414|-3.6138|-2.4951|

```matlab
% Matriz de sensibilidades dos trânsitos (PTDF): F = A*P
C = zeros(6,4);                              % incidência sem o slack
for l = 1:6
    if de(l) > 1, C(l,de(l)-1) =  1/X(l); end
    if pa(l) > 1, C(l,pa(l)-1) = -1/X(l); end
end
A  = C*Bi;
F0 = 100*A*P0;
Fa = F0 + 100*(A.*(A>0)*dPa + A.*(A<0)*dPc);
Fc = F0 + 100*(A.*(A>0)*dPc + A.*(A<0)*dPa);
F_DC = [Fa F0 Fc];                            % MW, colunas a | b | c


ln_F_DC  = {'1-2';'1-5';'2-3';'2-5';'3-4';'4-5'};
col_F_DC = {'a', 'b', 'c'};


disp_ang_DC = array2table(F_DC, 'RowNames', ln_F_DC, 'VariableNames', col_F_DC)
```

| |a|b|c|
|:--:|:--:|:--:|:--:|
|1 1-2|43.3913|60.2899|79.5942|
|2 1-5|13.6087|19.7101|26.4058|
|3 2-3|-16.8841|-8.9855|0.3333|
|4 2-5|4.2029|9.2754|14.3333|
|5 3-4|-56.7101|-48.9855|-42.8406|
|6 4-5|64.0290|71.0145|78.4203|