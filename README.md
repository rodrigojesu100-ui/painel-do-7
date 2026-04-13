wnz-dashboard-panel/
├── src/
│   ├── components/
│   │   ├── Sidebar.tsx
│   │   ├── Dashboard.tsx
│   │   ├── CardPanel.tsx
│   │   └── Navigation.tsx
│   ├── pages/
│   │   ├── Home.tsx
│   │   ├── Cards.tsx
│   │   └── Settings.tsx
│   ├── styles/
│   │   └── globals.css
│   ├── App.tsx
│   ├── index.tsx
│   └── types.ts
├── public/
│   └── index.html
├── package.json
├── tsconfig.json
├── tailwind.config.js
└── README.md
{
  "name": "wnz-dashboard-panel",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-scripts": "5.0.1",
    "tailwindcss": "^3.3.0",
    "typescript": "^4.9.5",
    "@types/react": "^18.0.0",
    "@types/react-dom": "^18.0.0",
    "lucide-react": "^0.263.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": ["react-app"]
  },
  "browserslist": {
    "production": [">0.2%", "not dead", "not op_mini all"],
    "development": ["last 1 chrome version", "last 1 firefox version", "last 1 safari version"]
  }
  {
  "compilerOptions": {
    "target": "es5",
    "lib": ["es6", "dom"],
    "jsx": "react-jsx",
    "module": "esnext",
    "moduleResolution": "node",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true
  },
  "include": ["src"],
  "exclude": ["node_modules"]
}
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {
      colors: {
        wnz: {
          primary: "#1a1a2e",
          secondary: "#16213e",
          accent: "#0f3460",
          highlight: "#e94560"
        }
      }
    }
  },
  plugins: []
};
export interface Card {
  id: string;
  cardNumber: string;
  cardName: string;
  expiryDate: string;
  cvv: string;
  bank: string;
  balance: number;
  color: string;
}

export interface DashboardMetrics {
  totalBalance: number;
  totalCards: number;
  monthlySpent: number;
  transactions: number;
}

export interface NavItem {
  id: string;
  label: string;
  icon: string;
  path: string;
}
import React, { useState } from 'react';
import { Menu, X, Home, CreditCard, Settings, LogOut } from 'lucide-react';

interface SidebarProps {
  onNavigate: (page: string) => void;
  currentPage: string;
}

export const Sidebar: React.FC<SidebarProps> = ({ onNavigate, currentPage }) => {
  const [isOpen, setIsOpen] = useState(true);

  const menuItems = [
    { id: 'home', label: 'Dashboard', icon: Home, path: 'home' },
    { id: 'cards', label: 'Meus Cartões', icon: CreditCard, path: 'cards' },
    { id: 'settings', label: 'Configurações', icon: Settings, path: 'settings' },
  ];

  const handleNavigate = (path: string) => {
    onNavigate(path);
  };

  return (
    <>
      {/* Toggle Button */}
      <button
        onClick={() => setIsOpen(!isOpen)}
        className="fixed top-4 left-4 z-50 p-2 rounded-lg bg-wnz-primary border border-wnz-accent md:hidden"
      >
        {isOpen ? <X size={24} className="text-wnz-highlight" /> : <Menu size={24} className="text-wnz-highlight" />}
      </button>

      {/* Sidebar */}
      <aside
        className={`fixed left-0 top-0 h-screen w-64 bg-gradient-to-b from-wnz-primary to-wnz-secondary border-r border-wnz-accent transition-transform duration-300 ${
          isOpen ? 'translate-x-0' : '-translate-x-full'
        } md:translate-x-0 z-40`}
      >
        {/* Logo */}
        <div className="p-6 border-b border-wnz-accent">
          <h1 className="text-3xl font-bold text-wnz-highlight">WNZ</h1>
          <p className="text-xs text-gray-400 mt-2">Dashboard Profissional</p>
        </div>

        {/* Menu Items */}
        <nav className="p-4 space-y-2">
          {menuItems.map((item) => {
            const Icon = item.icon;
            const isActive = currentPage === item.path;
            
            return (
              <button
                key={item.id}
                onClick={() => {
                  handleNavigate(item.path);
                  setIsOpen(false);
                }}
                className={`w-full flex items-center gap-3 px-4 py-3 rounded-lg transition-all ${
                  isActive
                    ? 'bg-wnz-accent text-wnz-highlight border-l-4 border-wnz-highlight'
                    : 'text-gray-300 hover:bg-wnz-accent hover:text-white'
                }`}
              >
                <Icon size={20} />
                <span className="font-medium">{item.label}</span>
              </button>
            );
          })}
        </nav>

        {/* Footer */}
        <div className="absolute bottom-0 left-0 right-0 p-4 border-t border-wnz-accent">
          <button className="w-full flex items-center gap-3 px-4 py-3 text-gray-300 hover:text-wnz-highlight rounded-lg transition-colors">
            <LogOut size={20} />
            <span>Sair</span>
          </button>
        </div>
      </aside>

      {/* Overlay */}
      {isOpen && (
        <div
          className="fixed inset-0 bg-black/50 md:hidden z-30"
          onClick={() => setIsOpen(false)}
        />
      )}
    </>
  );
};
import React from 'react';
import { TrendingUp, CreditCard, Send, ArrowRight } from 'lucide-react';
import { DashboardMetrics } from '../types';

interface DashboardProps {
  metrics: DashboardMetrics;
  onViewCards: () => void;
}

export const Dashboard: React.FC<DashboardProps> = ({ metrics, onViewCards }) => {
  const cards = [
    {
      id: 1,
      number: '**** **** **** 4512',
      bank: 'Banco do Brasil',
      color: 'from-yellow-600 to-yellow-400',
      balance: 5234.50
    },
    {
      id: 2,
      number: '**** **** **** 8234',
      bank: 'Itaú',
      color: 'from-blue-600 to-blue-400',
      balance: 3421.75
    },
    {
      id: 3,
      number: '**** **** **** 1567',
      bank: 'Bradesco',
      color: 'from-red-600 to-red-400',
      balance: 2156.30
    }
  ];

  return (
    <div className="flex-1 md:ml-64 p-4 md:p-8 bg-gradient-to-br from-gray-900 to-black min-h-screen">
      {/* Header */}
      <div className="mb-8 pt-12 md:pt-0">
        <h2 className="text-4xl font-bold text-white mb-2">Bem-vindo ao WNZ</h2>
        <p className="text-gray-400">Painel de controle de cartões e transações</p>
      </div>

      {/* Metrics Grid */}
      <div className="grid grid-cols-1 md:grid-cols-4 gap-4 mb-8">
        {[
          { label: 'Saldo Total', value: `R$ ${metrics.totalBalance.toFixed(2)}`, icon: CreditCard, color: 'from-blue-500 to-blue-600' },
          { label: 'Total de Cartões', value: metrics.totalCards, icon: CreditCard, color: 'from-purple-500 to-purple-600' },
          { label: 'Gasto Mensal', value: `R$ ${metrics.monthlySpent.toFixed(2)}`, icon: Send, color: 'from-pink-500 to-pink-600' },
          { label: 'Transações', value: metrics.transactions, icon: TrendingUp, color: 'from-green-500 to-green-600' }
        ].map((metric, index) => {
          const Icon = metric.icon;
          return (
            <div
              key={index}
              className={`bg-gradient-to-br ${metric.color} p-6 rounded-xl shadow-lg text-white`}
            >
              <div className="flex items-center justify-between">
                <div>
                  <p className="text-gray-100 text-sm">{metric.label}</p>
                  <p className="text-2xl font-bold mt-2">{metric.value}</p>
                </div>
                <Icon size={40} className="opacity-80" />
              </div>
            </div>
          );
        })}
      </div>

      {/* Cards Section */}
      <div className="mb-8">
        <div className="flex justify-between items-center mb-4">
          <h3 className="text-2xl font-bold text-white">Meus Cartões</h3>
          <button
            onClick={onViewCards}
            className="text-wnz-highlight hover:text-white transition-colors flex items-center gap-2"
          >
            Ver todos <ArrowRight size={18} />
          </button>
        </div>

        <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
          {cards.map((card) => (
            <div
              key={card.id}
              className={`bg-gradient-to-br ${card.color} p-8 rounded-2xl shadow-2xl text-white h-56 flex flex-col justify-between transform hover:scale-105 transition-transform cursor-pointer`}
            >
              <div>
                <p className="text-sm opacity-80">{card.bank}</p>
                <p className="text-2xl font-mono mt-4">{card.number}</p>
              </div>
              <div className="flex justify-between items-end">
                <div>
                  <p className="text-xs opacity-80">Saldo Disponível</p>
                  <p className="text-xl font-bold">R$ {card.balance.toFixed(2)}</p>
                </div>
                <CreditCard size={40} className="opacity-80" />
              </div>
            </div>
          ))}
        </div>
      </div>

      {/* Recent Transactions */}
      <div className="bg-wnz-secondary border border-wnz-accent rounded-xl p-6">
        <h3 className="text-xl font-bold text-white mb-4">Transações Recentes</h3>
        <div className="space-y-3">
          {[
            { desc: 'Compra Online', value: '-R$ 234.50', date: 'Hoje', status: 'Concluído' },
            { desc: 'Transferência Recebida', value: '+R$ 1.200.00', date: 'Ontem', status: 'Concluído' },
            { desc: 'Pagamento de Conta', value: '-R$ 450.00', date: '2 dias atrás', status: 'Concluído' }
          ].map((tx, index) => (
            <div key={index} className="flex justify-between items-center p-3 bg-wnz-primary rounded-lg border border-wnz-accent/20">
              <div>
                <p className="text-white font-medium">{tx.desc}</p>
                <p className="text-gray-400 text-sm">{tx.date}</p>
              </div>
              <div className="text-right">
                <p className={`font-bold ${tx.value.startsWith('+') ? 'text-green-400' : 'text-red-400'}`}>
                  {tx.value}
                </p>
                <p className="text-gray-400 text-xs">{tx.status}</p>
              </div>
            </div>
          ))}
        </div>
      </div>
    </div>
  );
};
import React, { useState } from 'react';
import { Plus, Copy, Trash2, Edit2, Eye, EyeOff } from 'lucide-react';
import { Card } from '../types';

export const CardPanel: React.FC = () => {
  const [cards, setCards] = useState<Card[]>([
    {
      id: '1',
      cardNumber: '4532123456789012',
      cardName: 'Cartão Principal',
      expiryDate: '12/26',
      cvv: '123',
      bank: 'Banco do Brasil',
      balance: 5234.50,
      color: 'from-yellow-600 to-yellow-400'
    },
    {
      id: '2',
      cardNumber: '5425000000000001',
      cardName: 'Cartão Secundário',
      expiryDate: '08/25',
      cvv: '456',
      bank: 'Itaú',
      balance: 3421.75,
      color: 'from-blue-600 to-blue-400'
    }
  ]);

  const [showModal, setShowModal] = useState(false);
  const [selectedCard, setSelectedCard] = useState<Card | null>(null);
  const [showCVV, setShowCVV] = useState<{ [key: string]: boolean }>({});
  const [isCloning, setIsCloning] = useState(false);

  const handleCloneCard = (card: Card) => {
    setIsCloning(true);
    setSelectedCard(card);
    
    setTimeout(() => {
      const newCard: Card = {
        ...card,
        id: Date.now().toString(),
        cardName: `${card.cardName} (Clonado)`
      };
      setCards([...cards, newCard]);
      setIsCloning(false);
    }, 600);
  };

  const handleDeleteCard = (id: string) => {
    setCards(cards.filter(card => card.id !== id));
  };

  const maskCardNumber = (cardNumber: string) => {
    return `**** **** **** ${cardNumber.slice(-4)}`;
  };

  const toggleCVVVisibility = (id: string) => {
    setShowCVV(prev => ({
      ...prev,
      [id]: !prev[id]
    }));
  };

  return (
    <div className="flex-1 md:ml-64 p-4 md:p-8 bg-gradient-to-br from-gray-900 to-black min-h-screen">
      {/* Header */}
      <div className="mb-8 pt-12 md:pt-0">
        <div className="flex justify-between items-center">
          <div>
            <h2 className="text-4xl font-bold text-white mb-2">Painel de Cartões</h2>
            <p className="text-gray-400">Gerencie e clone seus cartões</p>
          </div>
          <button
            onClick={() => setShowModal(true)}
            className="bg-wnz-highlight hover:bg-red-600 text-white px-6 py-3 rounded-lg flex items-center gap-2 transition-colors"
          >
            <Plus size={20} />
            Novo Cartão
          </button>
        </div>
      </div>

      {/* Cards Grid */}
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 mb-8">
        {cards.map((card) => (
          <div
            key={card.id}
            className={`bg-gradient-to-br ${card.color} p-8 rounded-2xl shadow-2xl text-white relative overflow-hidden transition-all hover:shadow-3xl ${
              selectedCard?.id === card.id && isCloning ? 'animate-pulse' : ''
            }`}
          >
            {/* Card Header */}
            <div className="flex justify-between items-start mb-8">
              <div>
                <p className="text-xs opacity-75">Banco</p>
                <p className="text-lg font-semibold">{card.bank}</p>
              </div>
              <CreditCardIcon />
            </div>

            {/* Card Number */}
            <div className="mb-6">
              <p className="text-xs opacity-75 mb-2">Número do Cartão</p>
              <p className="text-2xl font-mono tracking-wider">{maskCardNumber(card.cardNumber)}</p>
            </div>

            {/* Card Details */}
            <div className="flex justify-between mb-8">
              <div>
                <p className="text-xs opacity-75">Titular</p>
                <p className="font-semibold">{card.cardName}</p>
              </div>
              <div>
                <p className="text-xs opacity-75">Vencimento</p>
                <p className="font-semibold">{card.expiryDate}</p>
              </div>
            </div>

            {/* CVV */}
            <div className="mb-6 bg-black/30 p-3 rounded-lg">
              <div className="flex justify-between items-center">
                <div>
                  <p className="text-xs opacity-75">CVV</p>
                  <p className="text-lg font-mono">
                    {showCVV[card.id] ? card.cvv : '***'}
                  </p>
                </div>
                <button
                  onClick={() => toggleCVVVisibility(card.id)}
                  className="p-2 hover:bg-white/20 rounded-lg transition-colors"
                >
                  {showCVV[card.id] ? <EyeOff size={16} /> : <Eye size={16} />}
                </button>
              </div>
            </div>

            {/* Balance */}
            <div className="mb-6">
              <p className="text-xs opacity-75">Saldo Disponível</p>
              <p className="text-2xl font-bold">R$ {card.balance.toFixed(2)}</p>
            </div>

            {/* Actions */}
            <div className="flex gap-2 pt-6 border-t border-white/20">
              <button
                onClick={() => {
                  navigator.clipboard.writeText(card.cardNumber);
                  alert('Número copiado!');
                }}
                className="flex-1 bg-black/40 hover:bg-black/60 p-2 rounded-lg flex items-center justify-center gap-2 transition-colors text-sm"
              >
                <Copy size={16} />
                Copiar
              </button>
              <button
                onClick={() => handleCloneCard(card)}
                className="flex-1 bg-black/40 hover:bg-black/60 p-2 rounded-lg flex items-center justify-center gap-2 transition-colors text-sm"
                disabled={isCloning}
              >
                <Copy size={16} />
                Clonar
              </button>
              <button
                onClick={() => handleDeleteCard(card.id)}
                className="flex-1 bg-red-500/40 hover:bg-red-600 p-2 rounded-lg flex items-center justify-center gap-2 transition-colors text-sm"
              >
                <Trash2 size={16} />
                Deletar
              </button>
            </div>
          </div>
        ))}
      </div>

      {/* Cards Stats */}
      <div className="bg-wnz-secondary border border-wnz-accent rounded-xl p-6">
        <h3 className="text-xl font-bold text-white mb-4">Estatísticas</h3>
        <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
          <div className="bg-wnz-primary p-4 rounded-lg border border-wnz-accent/20">
            <p className="text-gray-400 text-sm">Total de Cartões</p>
            <p className="text-3xl font-bold text-white mt-2">{cards.length}</p>
          </div>
          <div className="bg-wnz-primary p-4 rounded-lg border border-wnz-accent/20">
            <p className="text-gray-400 text-sm">Saldo Total</p>
            <p className="text-3xl font-bold text-green-400 mt-2">
              R$ {cards.reduce((sum, card) => sum + card.balance, 0).toFixed(2)}
            </p>
          </div>
          <div className="bg-wnz-primary p-4 rounded-lg border border-wnz-accent/20">
            <p className="text-gray-400 text-sm">Cartões Ativos</p>
            <p className="text-3xl font-bold text-blue-400 mt-2">{cards.length}</p>
          </div>
        </div>
      </div>
    </div>
  );
};

const CreditCardIcon = () => (
  <svg
    className="w-8 h-8 opacity-90"
    fill="currentColor"
    viewBox="0 0 20 20"
  >
    <path d="M4 4a2 2 0 00-2 2v8a2 2 0 002 2h12a2 2 0 002-2V6a2 2 0 00-2-2H4zm0 2h12v2H4V6zm0 4h12v2H4v-2z" />
  </svg>
);
import React, { useState } from 'react';
import { Sidebar } from './components/Sidebar';
import { Dashboard } from './components/Dashboard';
import { CardPanel } from './components/CardPanel';
import { DashboardMetrics } from './types';

function App() {
  const [currentPage, setCurrentPage] = useState('home');

  const metrics: DashboardMetrics = {
    totalBalance: 10812.55,
    totalCards: 3,
    monthlySpent: 2456.78,
    transactions: 45
  };

  const renderPage = () => {
    switch (currentPage) {
      case 'home':
        return (
          <Dashboard
            metrics={metrics}
            onViewCards={() => setCurrentPage('cards')}
          />
        );
      case 'cards':
        return <CardPanel />;
      case 'settings':
        return (
          <div className="flex-1 md:ml-64 p-8 bg-gradient-to-br from-gray-900 to-black min-h-screen">
            <div className="pt-12 md:pt-0">
              <h2 className="text-4xl font-bold text-white mb-8">Configurações</h2>
              <div className="bg-wnz-secondary border border-wnz-accent rounded-xl p-8 max-w-2xl">
                <div className="space-y-6">
                  <div className="pb-6 border-b border-wnz-accent/20">
                    <h3 className="text-xl font-bold text-white mb-2">Perfil</h3>
                    <p className="text-gray-400">Atualize suas informações pessoais</p>
                  </div>
                  <div className="pb-6 border-b border-wnz-accent/20">
                    <h3 className="text-xl font-bold text-white mb-2">Segurança</h3>
                    <p className="text-gray-400">Gerenciar senha e autenticação</p>
                  </div>
                  <div>
                    <h3 className="text-xl font-bold text-white mb-2">Notificações</h3>
                    <p className="text-gray-400">Configure alertas e notificações</p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        );
      default:
        return <Dashboard metrics={metrics} onViewCards={() => setCurrentPage('cards')} />;
    }
  };

  return (
    <div className="flex bg-black">
      <Sidebar onNavigate={setCurrentPage} currentPage={currentPage} />
      {renderPage()}
    </div>
  );
}

export default App;
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import './styles/globals.css';

const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);

root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
@tailwind base;
@tailwind components;
@tailwind utilities;

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: #000;
  color: #fff;
}

html {
  scroll-behavior: smooth;
}

::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: #1a1a2e;
}

::-webkit-scrollbar-thumb {
  background: #0f3460;
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: #e94560;
}

button {
  cursor: pointer;
}

@keyframes gradient {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}
<!DOCTYPE html>
<html lang="pt-BR">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#1a1a2e" />
    <meta
      name="description"
      content="WNZ Dashboard - Painel Profissional de Cartões"
    />
    <title>WNZ Dashboard</title>
  </head>
  <body>
    <noscript>Você precisa habilitar JavaScript para executar este app.</noscript>
    <div id="root"></div>
  </body>
</html>
# WNZ Dashboard Panel 🚀

Painel profissional de dashboard com gerenciamento de cartões, desenvolvido em React + TypeScript com Tailwind CSS.

## 🎯 Características

✅ Dashboard responsivo com métricas em tempo real
✅ Painel lateral navegável (Sidebar)
✅ Gerenciamento completo de cartões
✅ Sistema de clonagem de cartões
✅ Interface moderna e profissional
✅ Tema escuro WNZ
✅ Suporte mobile

## 📦 Instalação

```bash
# Clone o repositório
git clone https://github.com/rodrigojesu100-ui/wnz-dashboard-panel.git

# Acesse o diretório
cd wnz-dashboard-panel

# Instale as dependências
npm install

# Inicie o servidor de desenvolvimento
npm start
src/
├── components/
│   ├── Sidebar.tsx
│   ├── Dashboard.tsx
│   └── CardPanel.tsx
├── pages/
├── styles/
├── App.tsx
└── index.tsx

}
