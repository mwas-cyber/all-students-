```jsx
import React, { useState, useEffect } from 'react';
import { Users, FileText, MessageSquare, Shield, CreditCard, Settings, Moon, Sun, BookOpen, PenTool, Link, Bell, ChevronDown, Menu, X } from 'lucide-react';

const App = () => {
  const [currentUser, setCurrentUser] = useState('admin'); // 'student', 'writer', 'admin'
  const [darkMode, setDarkMode] = useState(false);
  const [mobileMenuOpen, setMobileMenuOpen] = useState(false);
  const [activeTab, setActiveTab] = useState('dashboard');

  // Mock data
  const assignments = [
    { id: 1, title: 'Biology Research Paper', subject: 'Biology', deadline: '2024-01-15', status: 'posted', price: 120 },
    { id: 2, title: 'Calculus Homework', subject: 'Mathematics', deadline: '2024-01-12', status: 'in-progress', price: 85 },
    { id: 3, title: 'Business Plan Draft', subject: 'Business', deadline: '2024-01-20', status: 'completed', price: 200 }
  ];

  const writers = [
    { id: 1, name: 'Sarah Johnson', rating: 4.9, completed: 47, earnings: 1250 },
    { id: 2, name: 'Michael Chen', rating: 4.7, completed: 32, earnings: 890 },
    { id: 3, name: 'Emma Rodriguez', rating: 4.8, completed: 51, earnings: 1420 }
  ];

  const notifications = [
    'New assignment posted: Biology Research Paper',
    'Writer Michael Chen completed Calculus Homework',
    'Payment of $85 processed for completed assignment'
  ];

  // Role-based navigation
  const getNavigation = () => {
    if (currentUser === 'student') {
      return [
        { name: 'Dashboard', icon: FileText },
        { name: 'Post Assignment', icon: PenTool },
        { name: 'My Assignments', icon: BookOpen },
        { name: 'Wallet', icon: CreditCard },
        { name: 'Messages', icon: MessageSquare }
      ];
    } else if (currentUser === 'writer') {
      return [
        { name: 'Dashboard', icon: FileText },
        { name: 'Available Jobs', icon: BookOpen },
        { name: 'My Work', icon: PenTool },
        { name: 'Earnings', icon: CreditCard },
        { name: 'Messages', icon: MessageSquare }
      ];
    } else {
      return [
        { name: 'Dashboard', icon: FileText },
        { name: 'User Management', icon: Users },
        { name: 'Assignments', icon: BookOpen },
        { name: 'Payments', icon: CreditCard },
        { name: 'Messages', icon: MessageSquare },
        { name: 'Settings', icon: Settings }
      ];
    }
  };

  const NavigationItem = ({ item, active, onClick }) => {
    const Icon = item.icon;
    return (
      <button
        onClick={() => onClick(item.name)}
        className={`flex items-center space-x-3 w-full p-3 rounded-lg transition-colors ${
          active ? 'bg-blue-600 text-white' : 'text-gray-700 hover:bg-gray-100 dark:text-gray-300 dark:hover:bg-gray-700'
        }`}
      >
        <Icon size={20} />
        <span>{item.name}</span>
      </button>
    );
  };

  const AssignmentCard = ({ assignment }) => (
    <div className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-sm border border-gray-200 dark:border-gray-700">
      <div className="flex justify-between items-start mb-4">
        <div>
          <h3 className="font-semibold text-gray-900 dark:text-white">{assignment.title}</h3>
          <p className="text-gray-600 dark:text-gray-400 text-sm">{assignment.subject}</p>
        </div>
        <span className={`px-3 py-1 rounded-full text-xs font-medium ${
          assignment.status === 'posted' ? 'bg-yellow-100 text-yellow-800' :
          assignment.status === 'in-progress' ? 'bg-blue-100 text-blue-800' :
          'bg-green-100 text-green-800'
        }`}>
          {assignment.status.replace('-', ' ')}
        </span>
      </div>
      <div className="flex justify-between items-center">
        <span className="text-gray-500 dark:text-gray-400 text-sm">Due: {assignment.deadline}</span>
        <span className="font-semibold text-blue-600 dark:text-blue-400">${assignment.price}</span>
      </div>
    </div>
  );

  const AdminDashboard = () => (
    <div className="space-y-6">
      <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
        <div className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-sm border border-gray-200 dark:border-gray-700">
          <div className="flex items-center justify-between">
            <div>
              <p className="text-gray-600 dark:text-gray-400 text-sm">Total Students</p>
              <p className="text-2xl font-bold text-gray-900 dark:text-white">1,247</p>
            </div>
            <Users className="text-blue-500" size={24} />
          </div>
        </div>
        <div className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-sm border border-gray-200 dark:border-gray-700">
          <div className="flex items-center justify-between">
            <div>
              <p className="text-gray-600 dark:text-gray-400 text-sm">Verified Writers</p>
              <p className="text-2xl font-bold text-gray-900 dark:text-white">342</p>
            </div>
            <PenTool className="text-green-500" size={24} />
          </div>
        </div>
        <div className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-sm border border-gray-200 dark:border-gray-700">
          <div className="flex items-center justify-between">
            <div>
              <p className="text-gray-600 dark:text-gray-400 text-sm">Pending Reviews</p>
              <p className="text-2xl font-bold text-gray-900 dark:text-white">23</p>
            </div>
            <Shield className="text-orange-500" size={24} />
          </div>
        </div>
      </div>

      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <div className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-sm border border-gray-200 dark:border-gray-700">
          <h3 className="font-semibold text-lg text-gray-900 dark:text-white mb-4">Recent Assignments</h3>
          <div className="space-y-4">
            {assignments.slice(0, 3).map(assignment => (
              <AssignmentCard key={assignment.id} assignment={assignment} />
            ))}
          </div>
        </div>

        <div className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-sm border border-gray-200 dark:border-gray-700">
          <h3 className="font-semibold text-lg text-gray-900 dark:text-white mb-4">Top Writers</h3>
          <div className="space-y-4">
            {writers.map(writer => (
              <div key={writer.id} className="flex items-center justify-between p-3 bg-gray-50 dark:bg-gray-700 rounded-lg">
                <div>
                  <p className="font-medium text-gray-900 dark:text-white">{writer.name}</p>
                  <p className="text-sm text-gray-600 dark:text-gray-400">Rating: {writer.rating} ⭐</p>
                </div>
                <div className="text-right">
                  <p className="font-semibold text-gray-900 dark:text-white">${writer.earnings}</p>
                  <p className="text-sm text-gray-600 dark:text-gray-400">{writer.completed} jobs</p>
                </div>
              </div>
            ))}
          </div>
        </div>
      </div>
    </div>
  );

  const StudentDashboard = () => (
    <div className="space-y-6">
      <div className="bg-gradient-to-r from-blue-600 to-blue-700 rounded-xl p-6 text-white">
        <h2 className="text-2xl font-bold mb-2">Welcome back, Student!</h2>
        <p className="opacity-90">Post your assignment and get a custom quote within 24 hours.</p>
      </div>

      <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-sm border border-gray-200 dark:border-gray-700">
          <h3 className="font-semibold text-lg text-gray-900 dark:text-white mb-4">Your Assignments</h3>
          <div className="space-y-4">
            {assignments.map(assignment => (
              <AssignmentCard key={assignment.id} assignment={assignment} />
            ))}
          </div>
        </div>

        <div className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-sm border border-gray-200 dark:border-gray-700">
          <h3 className="font-semibold text-lg text-gray-900 dark:text-white mb-4">How It Works</h3>
          <div className="space-y-4">
            <div className="flex items-start space-x-3">
              <div className="w-8 h-8 bg-blue-100 dark:bg-blue-900 rounded-full flex items-center justify-center flex-shrink-0">
                <span className="text-blue-600 dark:text-blue-300 font-bold text-sm">1</span>
              </div>
              <p className="text-gray-600 dark:text-gray-400">Post your assignment with details and deadline</p>
            </div>
            <div className="flex items-start space-x-3">
              <div className="w-8 h-8 bg-blue-100 dark:bg-blue-900 rounded-full flex items-center justify-center flex-shrink-0">
                <span className="text-blue-600 dark:text-blue-300 font-bold text-sm">2</span>
              </div>
              <p className="text-gray-600 dark:text-gray-400">Receive a custom quote from our admin team</p>
            </div>
            <div className="flex items-start space-x-3">
              <div className="w-8 h-8 bg-blue-100 dark:bg-blue-900 rounded-full flex items-center justify-center flex-shrink-0">
                <span className="text-blue-600 dark:text-blue-300 font-bold text-sm">3</span>
              </div>
              <p className="text-gray-600 dark:text-gray-400">Pay securely and we'll assign your work to a verified expert</p>
            </div>
            <div className="flex items-start space-x-3">
              <div className="w-8 h-8 bg-blue-100 dark:bg-blue-900 rounded-full flex items-center justify-center flex-shrink-0">
                <span className="text-blue-600 dark:text-blue-300 font-bold text-sm">4</span>
              </div>
              <p className="text-gray-600 dark:text-gray-400">Receive quality work after our admin verification</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  );

  const WriterDashboard = () => (
    <div className="space-y-6">
      <div className="bg-gradient-to-r from-green-600 to-green-700 rounded-xl p-6 text-white">
        <h2 className="text-2xl font-bold mb-2">Welcome back, Writer!</h2>
        <p className="opacity-90">Browse available assignments and start earning.</p>
      </div>

      <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-sm border border-gray-200 dark:border-gray-700">
          <h3 className="font-semibold text-lg text-gray-900 dark:text-white mb-4">Available Assignments</h3>
          <div className="space-y-4">
            {assignments.filter(a => a.status === 'posted').map(assignment => (
              <AssignmentCard key={assignment.id} assignment={assignment} />
            ))}
          </div>
        </div>

        <div className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-sm border border-gray-200 dark:border-gray-700">
          <h3 className="font-semibold text-lg text-gray-900 dark:text-white mb-4">Your Earnings</h3>
          <div className="text-center py-8">
            <p className="text-3xl font-bold text-gray-900 dark:text-white">$1,250</p>
            <p className="text-gray-600 dark:text-gray-400 mt-2">This month</p>
            <button className="mt-4 bg-blue-600 hover:bg-blue-700 text-white px-6 py-2 rounded-lg transition-colors">
              Withdraw Funds ($10 min)
            </button>
            <p className="text-sm text-gray-500 dark:text-gray-400 mt-2">
              Payouts processed every Friday at midnight
            </p>
          </div>
        </div>
      </div>
    </div>
  );

  const renderDashboard = () => {
    switch (currentUser) {
      case 'admin':
        return <AdminDashboard />;
      case 'student':
        return <StudentDashboard />;
      case 'writer':
        return <WriterDashboard />;
      default:
        return <AdminDashboard />;
    }
  };

  return (
    <div className={`min-h-screen ${darkMode ? 'dark bg-gray-900' : 'bg-gray-50'}`}>
      {/* Header */}
      <header className="bg-white dark:bg-gray-800 shadow-sm border-b border-gray-200 dark:border-gray-700">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="flex justify-between items-center h-16">
            {/* Logo */}
            <div className="flex items-center space-x-3">
              <div className="bg-blue-600 p-2 rounded-lg">
                <BookOpen className="text-white" size={24} />
              </div>
              <div>
                <h1 className="text-xl font-bold text-gray-900 dark:text-white">StudyLink</h1>
                <p className="text-xs text-gray-500 dark:text-gray-400">Academic Excellence Platform</p>
              </div>
            </div>

            {/* Desktop Navigation */}
            <div className="hidden md:flex items-center space-x-6">
              <select 
                value={currentUser}
                onChange={(e) => setCurrentUser(e.target.value)}
                className="bg-gray-100 dark:bg-gray-700 text-gray-900 dark:text-white px-3 py-1 rounded-lg text-sm"
              >
                <option value="admin">Admin</option>
                <option value="student">Student</option>
                <option value="writer">Writer</option>
              </select>
              
              <button 
                onClick={() => setDarkMode(!darkMode)}
                className="p-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700"
              >
                {darkMode ? <Sun className="text-gray-400" size={20} /> : <Moon className="text-gray-600" size={20} />}
              </button>
              
              <button className="p-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700 relative">
                <Bell className="text-gray-600 dark:text-gray-400" size={20} />
                <span className="absolute -top-1 -right-1 bg-red-500 text-white text-xs rounded-full w-5 h-5 flex items-center justify-center">3</span>
              </button>
              
              <div className="flex items-center space-x-2">
                <div className="w-8 h-8 bg-blue-600 rounded-full flex items-center justify-center">
                  <span className="text-white font-semibold text-sm">A</span>
                </div>
                <span className="text-gray-700 dark:text-gray-300 font-medium">Admin</span>
                <ChevronDown className="text-gray-500 dark:text-gray-400" size={16} />
              </div>
            </div>

            {/* Mobile menu button */}
            <button 
              className="md:hidden p-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700"
              onClick={() => setMobileMenuOpen(!mobileMenuOpen)}
            >
              {mobileMenuOpen ? <X size={24} /> : <Menu size={24} />}
            </button>
          </div>
        </div>
      </header>

      <div className="flex">
        {/* Mobile Navigation */}
        {mobileMenuOpen && (
          <div className="md:hidden fixed inset-0 z-50 bg-black bg-opacity-50">
            <div className="absolute left-0 top-0 h-full w-64 bg-white dark:bg-gray-800 shadow-lg">
              <div className="p-4 border-b border-gray-200 dark:border-gray-700">
                <div className="flex items-center space-x-3">
                  <div className="bg-blue-600 p-2 rounded-lg">
                    <BookOpen className="text-white" size={24} />
                  </div>
                  <h1 className="text-xl font-bold text-gray-900 dark:text-white">StudyLink</h1>
                </div>
              </div>
              <div className="p-4 space-y-2">
                {getNavigation().map((item) => (
                  <NavigationItem
                    key={item.name}
                    item={item}
                    active={activeTab === item.name}
                    onClick={(name) => {
                      setActiveTab(name);
                      setMobileMenuOpen(false);
                    }}
                  />
                ))}
              </div>
            </div>
          </div>
        )}

        {/* Sidebar - Desktop */}
        <aside className="hidden md:block w-64 bg-white dark:bg-gray-800 shadow-sm min-h-screen">
          <div className="p-6">
            <div className="space-y-2">
              {getNavigation().map((item) => (
                <NavigationItem
                  key={item.name}
                  item={item}
                  active={activeTab === item.name}
                  onClick={setActiveTab}
                />
              ))}
            </div>
          </div>
        </aside>

        {/* Main Content */}
        <main className="flex-1 p-6">
          {renderDashboard()}
        </main>
      </div>
    </div>
  );
};

export default App;
```
