<!DOCTYPE html><html><head><meta charset="UTF-8"><script src="https://cdn.jsdelivr.net/npm/chart.js"></script><style>
        :root {
            --primario: #1a237e; --acento: #3d5afe; --fondo: #f8fafc;
            --blanco: #ffffff; --texto: #1e293b;
            --sob: #10b981; --nor: #f59e0b; --baj: #f97316; --cri: #ef4444;
        }

        body { font-family: 'Inter', 'Segoe UI', sans-serif; background: var(--fondo); color: var(--texto); margin: 0; }
        
        .auth-screen { position: fixed; inset: 0; background: linear-gradient(135deg, var(--primario), var(--acento)); display: flex; justify-content: center; align-items: center; z-index: 2000; }
        .auth-card { background: var(--blanco); padding: 40px; border-radius: 24px; width: 380px; text-align: center; box-shadow: 0 20px 25px -5px rgba(0,0,0,0.1); }
        .auth-card input { width: 100%; padding: 14px; margin: 10px 0; border: 1.5px solid #e2e8f0; border-radius: 12px; font-size: 1rem; }
        .btn-login { background: var(--acento); color: white; border: none; width: 100%; padding: 14px; border-radius: 12px; cursor: pointer; font-weight: 700; }

        .app { padding: 30px; max-width: 1500px; margin: auto; }
        .header { background: var(--blanco); padding: 20px; border-radius: 20px; display: flex; justify-content: space-between; align-items: center; margin-bottom: 25px; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.05); }

        .creds-top-panel { background: #f0fdf4; border: 1px solid #bbf7d0; padding: 20px; border-radius: 20px; margin-bottom: 25px; display: none; }
        .creds-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 10px; max-height: 200px; overflow-y: auto; padding: 10px; background: white; border-radius: 12px; }
        .cred-item { font-size: 0.8rem; border-bottom: 1px solid #f1f5f9; padding: 5px; }

        .admin-box { background: #eef2ff; padding: 20px; border-radius: 20px; margin-bottom: 25px; border: 2px dashed var(--acento); display: none; text-align: center; }
        
        .tabs { display: flex; gap: 15px; overflow-x: auto; padding-bottom: 15px; margin-bottom: 25px; }
        .tab-wrapper { background: var(--blanco); padding: 15px; border-radius: 18px; min-width: 180px; border: 2px solid transparent; box-shadow: 0 4px 6px rgba(0,0,0,0.05); transition: 0.3s; text-align: center; }
        .tab-wrapper.active { border-color: var(--acento); background: #f0f3ff; }
        .tab-actions { display: flex; justify-content: center; gap: 8px; margin-top: 10px; }
        
        .btn-icon { border: none; padding: 8px; border-radius: 8px; cursor: pointer; font-size: 1rem; transition: 0.2s; }
        .btn-view { background: var(--acento); color: white; width: 100%; margin-bottom: 5px; font-weight: bold; padding: 8px; border-radius: 8px; border: none; cursor: pointer; }
        .btn-exp { background: #e0e7ff; color: var(--primario); }
        .btn-del { background: #fee2e2; color: var(--cri); }

        .stats-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 15px; margin-bottom: 25px; }
        .stat-card { background: var(--blanco); padding: 20px; border-radius: 18px; text-align: center; box-shadow: 0 4px 6px rgba(0,0,0,0.05); }
        .stat-card h5 { margin: 0; color: #64748b; text-transform: uppercase; font-size: 0.75rem; }
        .stat-card .val { font-size: 1.8rem; font-weight: 800; color: var(--primario); margin: 5px 0; }
        .stat-card .desv { font-size: 0.7rem; color: var(--cri); font-weight: 700; background: #fff1f2; padding: 2px 8px; border-radius: 10px; display: inline-block; }

        .charts { display: grid; grid-template-columns: 1fr 2.5fr; gap: 25px; margin-bottom: 25px; }
        .chart-card { background: var(--blanco); padding: 25px; border-radius: 24px; box-shadow: 0 10px 15px rgba(0,0,0,0.05); min-height: 400px; }

        .card-table { background: var(--blanco); padding: 25px; border-radius: 24px; box-shadow: 0 10px 15px rgba(0,0,0,0.05); }
        table { width: 100%; border-collapse: collapse; }
        th { padding: 12px; font-size: 0.75rem; color: #94a3b8; text-transform: uppercase; border-bottom: 1px solid #f1f5f9; }
        td { padding: 12px; border-bottom: 1px solid #f8fafc; text-align: center; font-size: 0.9rem; }

        .badge { padding: 5px 12px; border-radius: 99px; font-weight: 700; font-size: 0.65rem; }
        .sob { background: #d1fae5; color: #065f46; }
        .nor { background: #fef3c7; color: #92400e; }
        .baj { background: #ffedd5; color: #9a3412; }
        .cri { background: #fee2e2; color: #991b1b; }
     .creds-top-panel, button, input { display:none !important; }</style></head><body><div class="app"><h2>Reporte: Simulacro 3</h2><div id="statsGrid" class="stats-grid"></div><div class="charts"><div class="chart-card"><canvas id="pieChart"></canvas></div><div class="chart-card"><canvas id="barChart"></canvas></div></div><table><thead><tr><th>Pos</th><th>Nombre</th><th>LEC</th><th>MAT</th><th>SOC</th><th>NAT</th><th>ING</th><th>GLOBAL</th></tr></thead><tbody id="tableBody"></tbody></table></div><script>const d = [{"nombre":"BERROCAL ARRIETA LUIS MATEO","lec":67,"mat":98,"soc":95,"cie":73,"ing":48,"global":403,"nivel":{"n":"Sobresaliente","c":"sob"}},{"nombre":"RUIZ RODRIGUEZ VALENTINA","lec":64,"mat":68,"soc":68,"cie":67,"ing":43,"global":325,"nivel":{"n":"Sobresaliente","c":"sob"}},{"nombre":"MARIMON TUIRAN SARA ENA","lec":64,"mat":61,"soc":50,"cie":54,"ing":55,"global":286,"nivel":{"n":"Normal","c":"nor"}},{"nombre":"APARICIO MELENDREZ ANGELA MARÍA","lec":58,"mat":43,"soc":59,"cie":56,"ing":55,"global":271,"nivel":{"n":"Normal","c":"nor"}},{"nombre":"MENDEZ SIERRA EIDY LUZ","lec":42,"mat":43,"soc":48,"cie":48,"ing":48,"global":227,"nivel":{"n":"Bajo","c":"baj"}},{"nombre":"ARRIETA GONZALEZ LORENA","lec":36,"mat":45,"soc":61,"cie":40,"ing":35,"global":225,"nivel":{"n":"Bajo","c":"baj"}},{"nombre":"MORENO SANCHEZ EMANUEL","lec":47,"mat":36,"soc":43,"cie":33,"ing":38,"global":198,"nivel":{"n":"Crítico","c":"cri"}},{"nombre":"ORTIZ MADERA CAMILO ANDRES","lec":50,"mat":25,"soc":36,"cie":44,"ing":28,"global":190,"nivel":{"n":"Crítico","c":"cri"}},{"nombre":"JARAMILLO CAMAÑO JUAN PABLO","lec":42,"mat":36,"soc":34,"cie":38,"ing":33,"global":186,"nivel":{"n":"Crítico","c":"cri"}},{"nombre":"RUIZ ESTRADA JUAN DAVID","lec":50,"mat":34,"soc":30,"cie":29,"ing":30,"global":176,"nivel":{"n":"Crítico","c":"cri"}},{"nombre":"GASPAR SIERRA EMILY VALENTINA","lec":36,"mat":25,"soc":27,"cie":40,"ing":38,"global":163,"nivel":{"n":"Crítico","c":"cri"}}]; document.getElementById('tableBody').innerHTML = d.map((s,i)=>'<tr><td>#'+(i+1)+'</td><td>'+s.nombre+'</td><td>'+s.lec+'</td><td>'+s.mat+'</td><td>'+s.soc+'</td><td>'+s.cie+'</td><td>'+s.ing+'</td><td>'+s.global+'</td></tr>').join(''); const m = ['lec','mat','soc','cie','ing','global']; const labels = ['Lectura','Mates','Sociales','Ciencias','Inglés','Global']; document.getElementById('statsGrid').innerHTML = m.map((k,idx)=>{ const vs=d.map(z=>Number(z[k])); const av=vs.reduce((a,b)=>a+b,0)/vs.length; const ds=Math.sqrt(vs.map(x=>Math.pow(x-av,2)).reduce((a,b)=>a+b,0)/vs.length); return '<div class="stat-card"><h5>'+labels[idx]+'</h5><div class="val">'+Math.round(av)+'</div><div class="desv">σ: '+Math.round(ds)+'</div></div>' }).join(''); const lv={sob:0,nor:0,baj:0,cri:0}; d.forEach(s=>lv[s.nivel.c]++); new Chart(document.getElementById('pieChart'),{type:'doughnut',data:{labels:['Sob','Nor','Baj','Cri'],datasets:[{data:Object.values(lv),backgroundColor:['#10b981','#f59e0b','#f97316','#ef4444']}]}}); new Chart(document.getElementById('barChart'),{type:'bar',data:{labels:['LEC','MAT','SOC','NAT','ING'],datasets:[{data:['lec','mat','soc','cie','ing'].map(k=>Math.round(d.reduce((a,b)=>a+b[k],0)/d.length)),backgroundColor:['#FF6384','#36A2EB','#FFCE56','#4BC0C0','#9966FF'],borderRadius:10}]},options:{maintainAspectRatio:false}});</script></body></html>
