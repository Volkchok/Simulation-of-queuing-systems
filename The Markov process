import random 
import math

def simulate_markov_process(T_max=100000.0, λ=2.2, μ=0.5, n=4):
  """
  Моделирование СМО M/M/4/0 по Марковскому процессу 
  """
  # Инициализация
  current_state = 0 # S₀ - все каналы свободны 
  time = 0.0
  
  # Статистика
  state_times = [0.0] * (n + 1) # время в каждом состоянии 
  arrival_count = 0
  rejection_count = 0
  
  
  
  print("Начало моделирования по Марковскому процессу...")
  
  
  while time < T_max:
    # Шаг 1: Определяем возможные переходы из текущего состояния
    
    
    # Для состояния S_k возможны:
    # 1. Приход заявки -> S_{k+1} (если k < n) с интенсивностью λ 
    # 2. Уход заявки -> S_{k-1} (если k > 0) с интенсивностью k*μ
    
    transitions = []
    
    
    # Переход вправо (приход) - если не в последнем состоянии 
    if current_state < n:
      # Генерируем время до прихода
      t_arrival = -math.log(random.random()) / λ 
      transitions.append(('arrival', t_arrival))
    
    # Переход влево (уход) - если не в первом состоянии 
    if current_state > 0:
      # Интенсивность ухода зависит от числа занятых каналов 
      departure_rate = current_state * μ
      t_departure = -math.log(random.random()) / departure_rate 
      transitions.append(('departure', t_departure))
    
  
    # Шаг 2: Находим переход, который произойдет раньше всех 
    if not transitions:
      break
    # Находим минимальное время до перехода 
    next_transition = min(transitions, key=lambda x: x[1]) 
    transition_type, min_time = next_transition
    
    # Шаг 3: Обновляем статистику пребывания в текущем состоянии 
    state_times[current_state] += min_time
    time += min_time
    
    
    # Шаг 4: Выполняем переход 
    if transition_type == 'arrival':
      arrival_count += 1 
      if current_state < n:
        current_state += 1 # принимаем заявку 
      else:
        rejection_count += 1 # отказ (хотя по логике не должно быть) 
    elif transition_type == 'departure':
      current_state -= 1 # освобождаем канал
    
  
  print(f"Моделирование завершено. Время: {time:.2f}")
  
  
  # Расчет характеристик total_time = sum(state_times)
  
  # Вероятности состояний
  P_states = [t/total_time for t in state_times]
  
  
  # Основные характеристики P_refusal = P_states[n] # P₄
  total_arrivals = arrival_count + rejection_count
  A = λ * (1 - P_refusal) # пропускная способность
  
  
  # Среднее число занятых каналов
  
  avg_busy = sum(k * P_states[k] for k in range(n + 1))
  
  
  # Остальные характеристики
  Q = 1 - P_refusal # относительная пропускная способность channel_util = avg_busy / n # коэффициент занятости
  
  return {
    'P_states': P_states, 
    'P_refusal': P_refusal, 
    'A': A,
    'avg_busy': avg_busy,
    'Q': Q,
    'channel_util': channel_util,
    'time': time,
    'arrival_count': arrival_count,
    'rejection_count': rejection_count,
    'total_arrivals': total_arrivals
  }
  
  
  
  
def compare_markov_with_analytical(results):
  
  """
  Сравнение результатов моделирования с аналитическими """
  # Аналитические значения для λ=2.2 
  analytic = {
    'P_refusal': 0.34784,
    'A': 1.4348,
    'avg_busy': 2.8696,
    'Q': 0.6522,
    'channel_util': 0.7174
  }
  
  
  names = {
    'P_refusal': 'Вероятность отказа',
    'A': 'Пропускная способность',
    'avg_busy': 'Среднее число занятых каналов',
    'Q': 'Относительная пропускная способность',
    'channel_util': 'Коэффициент занятости каналов'
  }
  
  
  print("\n" + "="*75)
  print("СРАВНЕНИЕ РЕЗУЛЬТАТОВ (Марковский процесс vs Аналитика)") 
  print("="*75)
  print(f"{'Характеристика':<40} | {'Аналитика':<10} | {'Моделирование':<12} | {'Отклонение':<10}") print("-"*75)
  
  for key in ['P_refusal', 'A', 'avg_busy', 'Q', 'channel_util']:
    anal_val = analytic[key] 
    model_val = results[key]
  
  if anal_val != 0:
    deviation = abs(model_val - anal_val) / anal_val * 100 
  else:
    deviation = 0
  
  
  print(f"{names[key]:<40} | {anal_val:>9.4f} | {model_val:>11.4f} | {deviation:>9.2f}%")
  print("="*75)
  
  
  # Вероятности состояний print("\nВероятности состояний:")
  for k, p in enumerate(results['P_states']):
    print(f" P{k} = {p:.5f}")
  
  
  

# Основная программа
if   name  == "  main  ":

  print("МОДЕЛИРОВАНИЕ СМО M/M/4/0 ПО МАРКОВСКОМУ ПРОЦЕССУ")
  print("=" * 50)
  
  print(f"Параметры: λ={2.2}, μ={0.5}, n=4, m=0")
  
  
  # Запуск моделирования
  results = simulate_markov_process(T_max=100000.0)
  
  
  # Вывод результатов
  print(f"\nРЕЗУЛЬТАТЫ МОДЕЛИРОВАНИЯ:")
  print(f"1. Вероятность отказа (P_отк): {results['P_refusal']:.4f} ({results['P_refusal']*100:.2f}%)") 
  print(f"2. Пропускная способность (A): {results['A']:.4f} заявок/время")
  print(f"3. Среднее число занятых каналов: {results['avg_busy']:.4f}") 
  print(f"4. Относительная пропускная способность (Q): {results['Q']:.4f}") 
  print(f"5. Коэффициент занятости каналов: {results['channel_util']:.4f}")
  
  
  # Сравнение с аналитикой 
  compare_markov_with_analytical(results)
  
  # Дополнительная статистика print(f"\nДополнительная информация:")
  print(f" Общее время моделирования: {results['time']:.2f}") 
  print(f" Прибыло заявок (по событиям): {results['arrival_count']}") 
  print(f" Доля времени в состояниях:")
  
  for k in range(5):
    print(f"  S{k}: {results['P_states'][k]:.3%}")
