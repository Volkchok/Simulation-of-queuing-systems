import random import math import heapq

def simulate_mm4_0_event_based(T_max=100000.0, λ=2.0, μ=0.5, n=4):
  """
  Моделирование СМО M/M/4/0 методом дискретных событий
  """
  # Инициализация
  time = 0.0
  busy_channels = 0
  total_requests = 0
  failed_requests = 0
  area_busy = 0.0
  last_event_time = 0.0 event_queue = []
  # Первое событие - прибытие
  first_arrival = -math.log(random.random()) / λ
  heapq.heappush(event_queue, (first_arrival, 'arrival'))
  
  
  # Главный цикл
  while time < T_max and event_queue:
    # Извлекаем ближайшее событие
    event_time, event_type = heapq.heappop(event_queue)
  
  
    # Обновляем интеграл статистики 
    time_delta = event_time - last_event_time 
    area_busy += busy_channels * time_delta 
    time = event_time
    last_event_time = time
    
    
    # Обрабатываем событие
    if event_type == 'arrival':
      total_requests += 1
      
      
      if busy_channels < n:
        # Принимаем заявку
        busy_channels += 1
        # Планируем уход
        service_time = -math.log(random.random()) / μ
        heapq.heappush(event_queue, (time + service_time, 'departure')) 
      else:
        # Отказ
        failed_requests += 1


      # Планируем следующее прибытие
      next_arrival = -math.log(random.random()) / λ
      heapq.heappush(event_queue, (time + next_arrival, 'arrival'))


    elif event_type == 'departure': 
      # Освобождаем канал 
      busy_channels -= 1

  # Финальное обновление статистики
  if time > 0:
    final_delta = time - last_event_time 
    area_busy += busy_channels * final_delta

  # Рассчитываем все характеристики
  P_refusal = failed_requests / total_requests if total_requests > 0 else 0 
  A = (total_requests - failed_requests) / time if time > 0 else 0
  avg_busy = area_busy / time if time > 0 else 0 Q = 1 - P_refusal
  channel_util = avg_busy / n if n > 0 else 0


  return {
    'P_refusal': P_refusal, 'A': A,
    'avg_busy': avg_busy, 'Q': Q,
    'channel_util': channel_util, 'total_requests': total_requests, 'failed_requests': failed_requests, 'time': time
  }

  
  
  
  def compare_with_analytical(results):
  """
  Сравнение результатов моделирования с аналитическими
  """
  # Аналитические значения (из раздела 6)
  analytic = {
  'P_refusal': 32/103,		# ≈ 0.3107 'A': 142/103,	# ≈ 1.3786
  'avg_busy': 284/103,	# ≈ 2.7573
  'Q': 71/103,	# ≈ 0.6893
  'channel_util': 71/103	# ≈ 0.6893
  }
  
  
  # Названия характеристик для вывода
  names = {
    'P_refusal': 'Вероятность отказа',
    'A': 'Пропускная способность',
    'avg_busy': 'Среднее число занятых каналов',
    'Q': 'Относительная пропускная способность',
    'channel_util': 'Коэффициент занятости каналов'
  }
  
  
  # Вывод таблицы сравнения
  print("\n" + "="*75)
  print("СРАВНЕНИЕ РЕЗУЛЬТАТОВ МОДЕЛИРОВАНИЯ С АНАЛИТИЧЕСКИМИ РАСЧЕТАМИ")
  print("="*75)
  print(f"{'Характеристика':<40} | {'Аналитика':<10} | {'Моделирование':<12} | {'Отклонение':<10}")
  print("-"*75)
  
  
  for key in ['P_refusal', 'A', 'avg_busy', 'Q', 'channel_util']:
    anal_val = analytic[key] 
    model_val = results[key]
  
    # Рассчитываем процентное отклонение
    if anal_val != 0:
      deviation = abs(model_val - anal_val) / anal_val * 100 
    else:
      deviation = 0


    print(f"{names[key]:<40} | {anal_val:>9.4f} | {model_val:>11.4f} | {deviation:>9.2f}%")
  
  
  print("="*75)
  

# Дополнительная информация
print(f"\nДополнительная информация:")
print(f" Время моделирования: {results['time']:.2f}") 
print(f" Всего заявок: {results['total_requests']}") 
print(f" Отказано: {results['failed_requests']}")
print(f" Обслужено: {results['total_requests'] - results['failed_requests']}")




# Основная часть программы
if   name  == "  main  ":
  print("МОДЕЛИРОВАНИЕ СМО M/M/4/0 МЕТОДОМ 'ПО СОБЫТИЯМ'")
  print("=" * 50)


  # Запуск моделирования
  results = simulate_mm4_0_event_based(T_max=100000.0)


  # Вывод результатов моделирования
  print("\nРЕЗУЛЬТАТЫ МОДЕЛИРОВАНИЯ:")
  print(f"1. Вероятность отказа (P_отк): {results['P_refusal']:.4f} ({results['P_refusal']*100:.2f}%)") 
  print(f"2. Пропускная способность (A): {results['A']:.4f} заявок/время")
  print(f"3. Среднее число занятых каналов: {results['avg_busy']:.4f}") 
  print(f"4. Относительная пропускная способность (Q): {results['Q']:.4f}")
  print(f"5. Коэффициент занятости каналов: {results['channel_util']:.4f}")
  
  # Сравнение с аналитическими расчетами
  compare_with_analytical(results)
